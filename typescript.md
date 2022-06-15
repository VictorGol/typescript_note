# 1 基础类型

`ts`是`js`的超集，所有`js`基础的类型都包含在内

起步安装`npm install typescript -g`

查看`ts`版本`tsc -v`

`npm install @types/node -D`

`npm install ts-node -g`

编译`ts`文件`tsc 文件名`

运行`js`文件`node 文件名`

运行`ts`文件`ts-node 文件名`

```typescript
let str: string = 'TS' // 字符串
let str1: string = `web${str}` // 字符串：模板字符串
let num: number = 123 // 数字
let num1: number = NaN // 数字：非数
let num2: number = Infinity // 数字：无穷大
let num3: number = 0xf00d // 数字：十六进制
let num4: number = 0b1010 // 数字：二进制
let num5: number = 0o744 // 数字：八进制
let b: boolean = true //布尔
let b1: boolean = Boolean(1) //布尔：通过构造函数生成
let u: void = undefined // 空值：undefined
let u1: void = null // 空值：null

// 声明函数没有返回值
function fnVoid(): void { }

// 和上面的区别，void类型不可以给别的类型赋值，undefined类型和null类型可以
let v: undefined = undefined // 空值
let v1: null = null // 空值
```

# 2 任意类型

## `any`类型

```typescript
let anys: any = '小满zs'
// 可以被赋值任意类型
anys = 18
anys = true
anys = {}
anys = []
anys = Symbol('123')
```

## `unknow`类型

```typescript
let anys: unknown = '小满zs'
// 也可以被赋值任意类型
anys = 18
anys = true
anys = {}
anys = []
anys = Symbol('123')
```

## 区别

`let unknow: unknown = { a: 123 }`

`unknow`不可以使用`a`属性，但换成`any`的话就可以

`unknown`类型比`any`类型安全

`any`类型可以赋给其他类型，但`unknown`类型只能赋值给相同类型或者`any`类型

# 3 接口和对象类型

声明一个对象

```typescript
interface A {
    name: string
}

let obj: A = {
    name: '嗷嗷'
}
```

接口重名

```typescript
interface A {
    name: string
}
// 如果接口重名，会合并
interface A {
    age: number
}

let obj: A = {
    name: '嗷嗷',
    age: 18
}
```

可选式操作符

```typescript
interface Person {
    name: string,
    age?: number // 可选式操作符：此属性不是必须
}

let p: Person = {
    name: '张三',
}
```

任意属性

```typescript
interface Person {
    name: string,
    age?: number,
    [propName: string]: any // 表示对象里可以有任意属性
    // [propName: string]: string // 表明所有属性必须是string
    // [propName: string]: string | number // 联合类型
}

let p: Person = {
    name: '张三',
    abc: 123
}
```

只读

```typescript
interface Person {
    readonly name: string, // 表明name属性只读，不能赋值
    [propName: string]: any
}

let p: Person = {
    name: '张三'
}
```

接口里定义函数

```typescript
interface Person {
    name: string,
    [propName: string]: any,
    cb(): number
}

let p: Person = {
    name: '张三',
    cb: (): number => {
        return 123
    }
}
```

接口的继承

```typescript
interface A {
    name: string
}

// 接口B继承接口A，相当于将A合并到B
// 可以继承多个
interface B extends A {
    age: number
}

let p: B = {
    age: 18,
    name: ''
}
```

# 4 数组类型

定义数组类型

```typescript
let arr: number[] = [1, 2, 3]
let arr1: string[] = ['1', '2', '3']
let arr2: any[] = [1, '2', true]
```

通过泛型定义

```typescript
let arr: Array<number> = [1, 2, 3]
```

声明多维数组

```typescript
let arr: number[][] = [[1,2], [3,4], [5]]
let arr1: Array<Array<number>> = [[], [], []]
```

类数组：arguments类数组

```typescript
// IArguments 是 TypeScript 中定义好了的类型，它实际上就是：
// interface IArguments {
//     [index: number]: any;
//     length: number;
//     callee: Function;
// }

// arg:所有参数的集合,是个数组
function Arr(...args: any): void {
    //arguments不是普通数组
    // 类型“IArguments”缺少类型“number[]”的以下属性: pop, push, concat, join 及其他 27 项
    console.log(arguments); // [Arguments] { '0': 4, '1': 5, '2': 6 }
    let arr: IArguments = arguments
    console.log(arr);
}
Arr(4, 5, 6)
```

类数组是结构与数组十分相似（类数组相当于一个对象，key是数字，或者数字的字符串形式，并且拥有length属性）但是却没有数组那么丰富的内建方法，通常类数组可能还拥有一些别的属性。

接口描述数组

```typescript
// 一般用来定义类数组
interface ArrNumber {
    [index: number]: string
}

let arr: ArrNumber = ['1', '2', '3']
console.log(arr); // [ '1', '2', '3' ]
```

# 5 函数类型

声明一个函数

```typescript
const fn = function (name: string, age: number): string {
    return name + age
}
// 小括号外的string表示返回值类型
let a = fn('张三', 10000)
console.log(a); // 张三10000
```

形参默认值

```typescript
const fn = function (name: string, age: number = 30): string {
    return name + age
}

let a = fn('张三') // 可以不传age参数
console.log(a); // 张三30
```

形参可选

```typescript
const fn = function (name: string, age?: number): string {
    return name + age
}

let a = fn('张三')
console.log(a); // 张三undefined
```

用接口来约束类型，将参数变成对象

```typescript
interface User {
    name: string,
    age: number
}

const fn = function (user: User): User {
    return user
}

let a = fn({ name: '张三', age: 18 })
console.log(a); // { name: '张三', age: 18 }
```

函数重载

重载是方法名字相同，而参数不同，返回类型可以相同也可以不同。

如果参数类型不同，则参数类型应设置为 **any**。

参数数量不同你可以将不同的参数设置为可选。

```typescript
// 这两个是重载函数
function fn(params: number): void
function fn(params: string, params2: number): void
// 这个是执行函数
function fn(params: any, params2?: number): void {
    console.log(params);
    console.log(params2);
}

let a = fn(1) // 1     undefined
let b = fn('1', 1) // 1     1
```

定义剩余参数

```typescript
const fn = (array:number[],...items:any[]):any[] => {
       console.log(array,items)
       return items
}
 
let a:number[] = [1,2,3]
 
fn(a,'4','5','6') // [ 1, 2, 3 ] [ '4', '5', '6' ]
```

# 6 类型断言 | 联合类型 | 交叉类型

## 联合类型

声明变量

```typescript
let phone:number | string = 13913139999
phone = '010-82855080'
```

声明函数

```typescript
// 数字或布尔值传进来都行，都能收到布尔
let fn = function (type: number | boolean): boolean {
    // 双非转布尔
    return !!type
}
```

## 交叉类型

```typescript
interface People {
    name: string,
    age: number
}

interface Man {
    sex: number
}

// man的类型是People与Man的合并,也就是参数得有三个属性
const xiaoman = (man: People & Man) => {
    console.log(man);
}

xiaoman({ name: '小满', age: 108, sex: 1 }) //{ name: '小满', age: 108, sex: 1 }
```

## 类型断言

类型断言可不是改变类型，而是欺骗`typescript`编译器

`as`写法

```typescript
let fn = function(num:number | string):void{
    // console.log(num.length); // 不行，number类型没有length
    console.log((num as string).length);
}
```

`<>`泛型写法

```typescript
interface A {
    run: string
}

interface B {
    build: string
}

let fn = (type: A | B) => {
    // console.log(type.run); // 不行，类型“B”上不存在属性“run”
    console.log((<A>type).run);
}
```

任何类型可以临时断言成`any`类型

```typescript
(window as any).abc = 123
const fn = (type: any): boolean{
    return type as boolean
}
console.log(fn(1)); // 这里还是打印数字1，并不是布尔
```

# 7 内置对象

`JavaScript`中有很多内置对象，它们可以直接在`TypeScript`中当做定义好了的类型。

## `ECMAScript`的内置对象

`Boolean`、`Number`、`string`、`RegExp`、`Date`、`Error`

```typescript
// new出来的是对象
let b: Boolean = new Boolean(1)
console.log(b) // [Boolean: true]
let n: Number = new Number(true)
console.log(n) // [Number: 1]
let s: String = new String('哔哩哔哩关注小满zs')
console.log(s) // [String: '哔哩哔哩关注小满zs']
let d: Date = new Date()
console.log(d) // 2022-06-10T02:06:34.551Z
let r: RegExp = /^1/
console.log(r) // /^1/
let e: Error = new Error("error!")
console.log(e) // Error: error!
```

## `DOM`和`BOM`的内置对象

`Document`、`HTMLElement`、`Event`、`NodeList` 等

```typescript
let body: HTMLElement = document.body;
let allDiv: NodeList = document.querySelectorAll('div');
//读取div 这种需要类型断言 或者加个判断应为读不到返回null
let div:HTMLElement = document.querySelector('div') as HTMLDivElement
document.addEventListener('click', function (e: MouseEvent) {
    
});
//dom元素的映射表
interface HTMLElementTagNameMap {
    "a": HTMLAnchorElement;
    "abbr": HTMLElement;
    "address": HTMLElement;
    "applet": HTMLAppletElement;
    "area": HTMLAreaElement;
    "article": HTMLElement;
    "aside": HTMLElement;
    "audio": HTMLAudioElement;
    "b": HTMLElement;
    "base": HTMLBaseElement;
    "bdi": HTMLElement;
    "bdo": HTMLElement;
    "blockquote": HTMLQuoteElement;
    "body": HTMLBodyElement;
    "br": HTMLBRElement;
    "button": HTMLButtonElement;
    "canvas": HTMLCanvasElement;
    "caption": HTMLTableCaptionElement;
    "cite": HTMLElement;
    "code": HTMLElement;
    "col": HTMLTableColElement;
    "colgroup": HTMLTableColElement;
    "data": HTMLDataElement;
    "datalist": HTMLDataListElement;
    "dd": HTMLElement;
    "del": HTMLModElement;
    "details": HTMLDetailsElement;
    "dfn": HTMLElement;
    "dialog": HTMLDialogElement;
    "dir": HTMLDirectoryElement;
    "div": HTMLDivElement;
    "dl": HTMLDListElement;
    "dt": HTMLElement;
    "em": HTMLElement;
    "embed": HTMLEmbedElement;
    "fieldset": HTMLFieldSetElement;
    "figcaption": HTMLElement;
    "figure": HTMLElement;
    "font": HTMLFontElement;
    "footer": HTMLElement;
    "form": HTMLFormElement;
    "frame": HTMLFrameElement;
    "frameset": HTMLFrameSetElement;
    "h1": HTMLHeadingElement;
    "h2": HTMLHeadingElement;
    "h3": HTMLHeadingElement;
    "h4": HTMLHeadingElement;
    "h5": HTMLHeadingElement;
    "h6": HTMLHeadingElement;
    "head": HTMLHeadElement;
    "header": HTMLElement;
    "hgroup": HTMLElement;
    "hr": HTMLHRElement;
    "html": HTMLHtmlElement;
    "i": HTMLElement;
    "iframe": HTMLIFrameElement;
    "img": HTMLImageElement;
    "input": HTMLInputElement;
    "ins": HTMLModElement;
    "kbd": HTMLElement;
    "label": HTMLLabelElement;
    "legend": HTMLLegendElement;
    "li": HTMLLIElement;
    "link": HTMLLinkElement;
    "main": HTMLElement;
    "map": HTMLMapElement;
    "mark": HTMLElement;
    "marquee": HTMLMarqueeElement;
    "menu": HTMLMenuElement;
    "meta": HTMLMetaElement;
    "meter": HTMLMeterElement;
    "nav": HTMLElement;
    "noscript": HTMLElement;
    "object": HTMLObjectElement;
    "ol": HTMLOListElement;
    "optgroup": HTMLOptGroupElement;
    "option": HTMLOptionElement;
    "output": HTMLOutputElement;
    "p": HTMLParagraphElement;
    "param": HTMLParamElement;
    "picture": HTMLPictureElement;
    "pre": HTMLPreElement;
    "progress": HTMLProgressElement;
    "q": HTMLQuoteElement;
    "rp": HTMLElement;
    "rt": HTMLElement;
    "ruby": HTMLElement;
    "s": HTMLElement;
    "samp": HTMLElement;
    "script": HTMLScriptElement;
    "section": HTMLElement;
    "select": HTMLSelectElement;
    "slot": HTMLSlotElement;
    "small": HTMLElement;
    "source": HTMLSourceElement;
    "span": HTMLSpanElement;
    "strong": HTMLElement;
    "style": HTMLStyleElement;
    "sub": HTMLElement;
    "summary": HTMLElement;
    "sup": HTMLElement;
    "table": HTMLTableElement;
    "tbody": HTMLTableSectionElement;
    "td": HTMLTableDataCellElement;
    "template": HTMLTemplateElement;
    "textarea": HTMLTextAreaElement;
    "tfoot": HTMLTableSectionElement;
    "th": HTMLTableHeaderCellElement;
    "thead": HTMLTableSectionElement;
    "time": HTMLTimeElement;
    "title": HTMLTitleElement;
    "tr": HTMLTableRowElement;
    "track": HTMLTrackElement;
    "u": HTMLElement;
    "ul": HTMLUListElement;
    "var": HTMLElement;
    "video": HTMLVideoElement;
    "wbr": HTMLElement;
}
```

## 定义Promise

```typescript
// Promise<number>表示返回值类型是Promise，泛型是number
// 从源码里可以看到，在Promise后定义了泛型，then后第一个参数是回调函数，回调函数的参数是对应的泛型
function promise(): Promise<number> {
    return new Promise<number>((resolve, reject) => {
        resolve(1)
    })
}
promise().then(res => {
    console.log(res);
})
```

# 8 Class类

```typescript
// 声明一个类
class Person {
    name: string
    age: number
    sub: boolean
    // 构造器
    constructor(name: string, age: number, sub: boolean) {
        this.name = name
        this.age = age
    }
}

// 实例化一个类，new的时候实际上有调用构造器
new Person('小满', 22, false)
console.log(p); // Person { name: '小满', age: 22, sub: false }
```

## 类的修饰符

总共有三个 public private protected，不写默认是public

public：类的内部外部都能访问

private：只能在类的内部访问

protected：只能在类的内部和其子类中访问

```typescript
class Person {
    // 声明
    protected name: string
    private age: number
    public sub: boolean
    static aaa: string = '161614' // 静态属性，直接通过类名访问
    constructor(name: string, age: number, sub: boolean) {
        // this.name是实例里的name，不是上面的，上面知识声明
        this.name = name
        this.age = age
        this.sub = sub
        // Person.run() // 可行
        // this.run() // 不可行
    }
    // 静态属性和方法是用来暴露给外部使用的
    // 静态方法，直接通过类名访问，静态方法里面不能访问非静态变量，这个例子只能访问this.aaa
    // 因为这里的this指的是类，构造函数的this指的是实例，所以构造器里也不能调用静态方法和属性
    static run() {
        console.log('run方法', this.aaa);
        return '789'
    }
    // 静态函数之间可以互相调用
    static dev() {
        this.run()
        return 'dev'
    }
}

class Man extends Person {
    constructor() {
        // 调用父类的构造函数
        super('小满', 22, false)
    }
}
// 访问静态属性
console.log('访问静态属性', Person.aaa);
// 访问静态方法
console.log('访问静态方法', Person.run());

let p = new Person('小满', 22, false)
```

## 用接口约束类

```typescript
interface Person {
    run(type: boolean): boolean
}

interface H {
    set(): void
}

class A {
    params: string
    constructor(params) {
        this.params = params
    }
}

// 使用implements关键字来约束类,表明此类必须要有run和set，相当于实现接口
// 使用extends关键字继承，表明此类获得了父类的元素
class Man extends A implements Person, H {
    // constructor...... 这个需要，但是没写
    run(type: boolean): boolean {
        return type
    }
    set() { }
}
```

## 抽象类

```typescript
// 定义抽象类,不能被实例化
abstract class A {
    name: string
    constructor(name: string) {
        this.name = name
    }
    // 定义抽象类的函数，里面不能有具体的实现步骤
    abstract getName(): string
    // 正常函数，可以被子类直接使用
    setName(name: string) {
        this.name = name
    }
}

class B extends A {
    constructor() {
        super('小满')
    }
    getName(): string {
        return this.name
    }
}

let b = new B()
b.setName('大大')
console.log(b.getName()); // 大大
```

# 9 元组类型

如果需要一个固定大小的不同类型值的集合，我们需要使用元组。 

元组与集合的不同之处在于，元组中的元素类型可以是不同的，而且数量固定。元组的好处在于可以把多个元素作为一个单元传递。如果一个方法需要返回多个值，可以把这多个值作为元组返回，而不需要创建额外的类来表示。

```typescript
// 定义元组,得按顺序
let arr: [string, number] = ['小满', 22]
// 对于越界的元素他的类型被限制为 联合类型（就是你在元组中定义的类型）
// arr.push(false) // 不行
arr.push('999')
// 应用场景 例如定义execl返回的数据
let excel: [string, string, number, string][] = [
    ['title', 'name', 1, '123'],
    ['title', 'name', 1, '123'],
    ['title', 'name', 1, '123'],
    ['title', 'name', 1, '123'],
    ['title', 'name', 1, '123'],
]
```

# 10 枚举

## 数字枚举

```typescript
// 根据不同字符串返回不同数字
const fn = (type: string): number => {
    if (type === 'red') {
        return 0
    }
    if (type === 'green') {
        return 1
    }
    if (type === 'blue') {
        return 2
    }
    return -1
}
// 通过对象实现以上功能
let obj = {
    red: 0,
    green: 1,
    blue: 2
}
// 枚举是用字符串代表数值
// 定义枚举，默认从0开始,也可以定义初始值
enum Color {
    red,
    green,
    blue
}
console.log(Color); // { '0': 'red', '1': 'green', '2': 'blue', red: 0, green: 1, blue: 2 }
console.log(Color.red); // 0
console.log(Color.green); // 1
console.log(Color.blue); // 2
console.log(Color[0]); // red
console.log(Color[1]); // green
console.log(Color[2]); // blue
```

```typescript
enum Types{
   Red = 0,
   Green = 1,
   BLue = 2
}
//默认就是从0开始的 可以不写值
```

增长枚举

```typescript
// 定义的值之后开始递增
enum Color {
    red,
    green = 11,
    blue
}
console.log(Color.red); // 0
console.log(Color.green); // 11
console.log(Color.blue); // 12
```

## 字符串枚举

```typescript
// 字符串枚举必须全部定义
enum Color {
    red = 'redd',
    green = 'greenn',
    blue = 'bluee'
}
console.log(Color); // { red: 'redd', green: 'greenn', blue: 'bluee' }
console.log(Color.red); // redd
console.log(Color.green); // greenn
console.log(Color.blue); // bluee
```

## 异构枚举

字符串和数组混合

```typescript
// 字符串枚举必须全部定义
enum Color {
    red = 9,
    green = 'greenn',
    blue = 'bluee'
}
console.log(Color); // { '9': 'red', red: 9, green: 'greenn', blue: 'bluee' }
console.log(Color.red); // 9
console.log(Color.green); // greenn
console.log(Color.blue); // bluee
```

## 接口枚举

```typescript
// 字符串枚举必须全部定义
enum Color {
    red = 9,
    green = 'greenn'
}

interface A {
    red: Color.green
}

let obj: A = {
    red: Color.green
}

console.log(obj); // { red: 'greenn' }
```

## `const`枚举

正常枚举

```typescript
enum Types {
    success,
    fail
}
console.log(Types.success);
```

编译后

```javascript
var Types;
(function (Types) {
    Types[Types["success"] = 0] = "success";
    Types[Types["fail"] = 1] = "fail";
})(Types || (Types = {}));
console.log(Types.success);
```

`const`枚举

```typescript
const enum Types{
    success,
    fail
}
console.log(Types.success);
```

编译后

```javascript
console.log(0 /* Types.success */);
```

## 反向映射

```typescript
enum Enum {
   fall
}
let a = Enum.fall;
console.log(a); // 0
let nameOfA = Enum[a]; // 这里的a成了数字0
console.log(nameOfA); // fall
```

字符串不支持

# 11 类型推论|类型别名

类型推论

```typescript
// 推论str的类型是string
let str = '小满'
// 推论成any类型
let a
a = 10
a = 'sss'
```

类型别名

```typescript
// 给类型换个名字
type s = string | number
let str:s = '小满'
let num:s = 78

// 函数式类型别名
type cb = () => string
const fn: cb = () => '小满'
console.log(fn()); // 小满

// 字面量类型的类型别名
type T = 'off' | 'on'
let str1:T = 'on' // str1只能是'on'或者'off'
```

# 12 never类型

表示不应该存在的状态

```typescript
// type bbb = string & number // 此处的bbb是never类型

// 例子：永远不会返回值
function error(message: string): never {
    throw new Error(message)
}

// 例子：永远不会返回值
function loop(): never {
    while (true) { }
}

// 例子：switch...case...
interface A {
    type: '保安'
}
interface B {
    type: '草莓'
}
type All = A | B
function type(val: All) {
    switch (val.type) {
        case '保安':
            break
        case '草莓':
            break
        default:
            // 如果走到这里，说明出错了，never类型不能被赋值
            const check: never = val
            break
    }
}
```

# 13 symbol类型

`symbol`类型的值是通过`Symbol`构造函数创建的。

可以传递参做为唯一标识，只支持 string 和 number类型的参数

```typescript
let s: symbol = Symbol('we')
let s1: symbol = Symbol(123)
let n: symbol = Symbol()
console.log(n, s, s1); // Symbol() Symbol(we) Symbol(123)
```

Symbol的值是唯一的，因为内存地址指针位置不同

```typescript
const s1 = Symbol()
const s2 = Symbol()
// s1 === s2 =>false
```

```typescript
// 用在对象里作为唯一key
let s: symbol = Symbol('we')
let obj = {
    // 用[]表示里面是个变量
    [s]: 'value'
}
console.log(obj); // { [Symbol(we)]: 'value' }
console.log(obj[s]); // value
```

使用symbol定义的属性，是不能通过如下方式遍历拿到的

```typescript
const symbol1 = Symbol('666')
const symbol2 = Symbol('777')
const obj1= {
   [symbol1]: '小满',
   [symbol2]: '二蛋',
   age: 19,
   sex: '女'
}
// 1 for in 遍历
for (const key in obj1) {
   // 注意在console看key,是不是没有遍历到symbol1
   console.log(key) // age sex
}
// 2 Object.keys 遍历
Object.keys(obj1)
console.log(Object.keys(obj1)) // ['age','sex']
// 3 getOwnPropertyNames
console.log(Object.getOwnPropertyNames(obj1)) // ['age','sex']
// 4 JSON.stringfy
console.log(JSON.stringify(obj1)) // {"age":19,"sex":"女"}
```

如何拿到

```typescript
// 1 拿到具体的symbol 属性,对象中有几个就会拿到几个
Object.getOwnPropertySymbols(obj1)
console.log(Object.getOwnPropertySymbols(obj1)) // [ Symbol(19), Symbol(女) ]
// 2 es6 的 Reflect 拿到对象的所有属性
Reflect.ownKeys(obj1)
console.log(Reflect.ownKeys(obj1)) // [ 'age', 'sex', Symbol(666), Symbol(777) ]
```

## Symbol.iterator 迭代器 和 生成器 for of

看来是很重要的东西

### 迭代器

示例：生成一个迭代器

```typescript
// 声明一个数字数组
let arr: Array<number> = [4, 5, 6]
// arr[Symbol.iterator]是个函数,it是个对象，名叫Array Iterator，仅有方法next()
let it: Iterator<number> = arr[Symbol.iterator]()
console.log(it); // Object [Array Iterator] {}
// 从头开始遍历，调一次，往后遍历一次
console.log(it.next()); // { value: 4, done: false }
console.log(it.next()); // { value: 5, done: false }
console.log(it.next()); // { value: 6, done: false }
console.log(it.next()); // { value: undefined, done: true }
```

测试用例

```typescript
let arr: Array<number> = [4, 5, 6]
let set: Set<number> = new Set([1, 2, 3])
console.log(set); // Set(3) { 1, 2, 3 } 是个Set对象

type mapKeys = string | number
let map: Map<mapKeys, mapKeys> = new Map()
console.log(map); // Map(0) {} 是个Map对象
map.set('1', '王爷')
map.set('2', '皇上')
console.log(map); // Map(2) { '1' => '王爷', '2' => '皇上' }

function gen(erg: any) {
   // 生成迭代器
   let it: Iterator<any> = erg[Symbol.iterator]()
   // 初始化变量
   let next: any = it.next()
   // 如果还未完成遍历
   while (!next.done) {
      console.log(next);
      next = it.next()
   }
}
gen(arr) // { value: 4, done: false }{ value: 5, done: false }{ value: 6, done: false }
gen(set) // { value: 1, done: false }{ value: 2, done: false }{ value: 3, done: false }
gen(map) // { value: [ '1', '王爷' ], done: false } { value: [ '2', '皇上' ], done: false }
```

对象不支持迭代器

### 生成器

示例

```typescript
let arr: Array<number> = [4, 5, 6]
let set: Set<number> = new Set([1, 2, 3])

type mapKeys = string | number
let map: Map<mapKeys, mapKeys> = new Map()
map.set('1', '王爷')
map.set('2', '皇上')

// for of 会自己去调Symbol.itrator函数去把value给取出来
for (let item of set) {
   console.log(item);// 1 2 3
}
// for of 简单来说就是itrator的语法糖
for (let item of arr) {
   console.log(item);// 4 5 6
}
for (let item of map) {
   console.log(item);// [ '1', '王爷' ] [ '2', '皇上' ]
}
// 跟for in 的区别,遍历出索引,for in 是遍历数组的
for (let item in arr) {
   console.log(item); // 0 1 2
}
```

# 14 泛型

## 函数式泛型

```typescript
// 一般函数写法
function num(a: number, b: number): Array<number> {
   return [a, b]
}
let re = num(1, 2)
console.log(re); // [1,2]
// 如果想传入字符出返回字符串数组，想传入布尔返回布尔数组，想传入对象，返回对象数组
// 那么使用泛型，可避免函数写多次
// 语法：函数名后跟<参数类型>，参数类型随意写（例如T），使用的时候，把参数类型传给T
function add<T>(a: T, b: T): Array<T> {
   return [a, b]
}
add<number>(1, 3)
add<string>('1', '3')
// 简写，触发类型推论
add(1, 3)
add('1', '3')
// 函数要用到多个类型时
function sub<T, U>(a: T, b: U): Array<T | U> {
   let arr: Array<T | U> = [a, b]
   return arr
}
console.log(sub<number, string>(1, '我')); // [ 1, '我' ]
// 简写
sub(1, '我')
```

泛型约束

```typescript
interface Len {
   length: number
}

// 因为只是1个T的话，是没有length属性的，这样有length属性的参数传进来时，才能调用
function getLength<T extends Len>(arg: T) {
   return arg.length
}

getLength('gfghjk') // 字符串、数组等作为参数是可以的，数字就不可以
```

## 使用keyof约束对象

一个普通函数引起的问题

```typescript
function prop<T>(obj: T, key) {
   return obj[key]
}

let o = { a: 1, b: 2, c: 3 }

prop(o, 'a')
// 如果传一个没有的key，没有提示，所以出现了使用keyof来进行约束
prop(o, 'd')
```

约束

```typescript
// keyof获取对象的所有键，并返回联合类型，利用extends关键字约束K类型必须为联合类型的子类型
function prop<T, K extends keyof T>(obj: T, key: K) {
   return obj[key]
}
let o = { a: 1, b: 2, c: 3 }
prop(o, 'v') // 类型“"v"”的参数不能赋给类型“"a" | "b" | "c"”的参数。
```

## 泛型类

```typescript
// 在类名后写<参数类型>
class Sub<T> {
   attr: T[] = []
   add(a: T): T[] {
      return [a]
   }
}
let s = new Sub<number>()
console.log(s); // Sub { attr: [] }
s.attr = [1, 2, 3]
console.log(s); // Sub { attr: [ 1, 2, 3 ] }
console.log(s.add(555)); // [555]

let str = new Sub<string>()
str.attr = ['1', '2', '3']
console.log(str); // Sub { attr: [ '1', '2', '3' ] }
console.log(str.add('555')); // [ '555' ]
```

# 15 tsconfig.json配置文件

在新文件夹构建ts文件

`echo ''>index.ts`

生成tsconfig.json文件

`tsc -init`

配置项

```json
"compilerOptions": {
  "incremental": true, // TS编译器在第一次编译之后会生成一个存储编译信息的文件，第二次编译会在第一次的基础上进行增量编译，可以提高编译的速度
  "tsBuildInfoFile": "./buildFile", // 增量编译文件的存储位置
  "diagnostics": true, // 打印诊断信息 
  "target": "ES5", // 目标语言的版本
  "module": "CommonJS", // 生成代码的模板标准
  "outFile": "./app.js", // 将多个相互依赖的文件生成一个文件，可以用在AMD模块中，即开启时应设置"module": "AMD",
  "lib": ["DOM", "ES2015", "ScriptHost", "ES2019.Array"], // TS需要引用的库，即声明文件，es5 默认引用dom、es5、scripthost,如需要使用es的高级版本特性，通常都需要配置，如es8的数组新特性需要引入"ES2019.Array",
  "allowJS": true, // 允许编译器编译JS，JSX文件
  "checkJs": true, // 允许在JS文件中报错，通常与allowJS一起使用
  "outDir": "./dist", // 指定输出目录
  "rootDir": "./", // 指定输出文件目录(用于输出)，用于控制输出目录结构
  "declaration": true, // 生成声明文件，开启后会自动生成声明文件
  "declarationDir": "./file", // 指定生成声明文件存放目录
  "emitDeclarationOnly": true, // 只生成声明文件，而不会生成js文件
  "sourceMap": true, // 生成目标文件的sourceMap文件
  "inlineSourceMap": true, // 生成目标文件的inline SourceMap，inline SourceMap会包含在生成的js文件中
  "declarationMap": true, // 为声明文件生成sourceMap
  "typeRoots": [], // 声明文件目录，默认时node_modules/@types
  "types": [], // 加载的声明文件包
  "removeComments":true, // 删除注释 
  "noEmit": true, // 不输出文件,即编译后不会生成任何js文件
  "noEmitOnError": true, // 发送错误时不输出任何文件
  "noEmitHelpers": true, // 不生成helper函数，减小体积，需要额外安装，常配合importHelpers一起使用
  "importHelpers": true, // 通过tslib引入helper函数，文件必须是模块
  "downlevelIteration": true, // 降级遍历器实现，如果目标源是es3/5，那么遍历器会有降级的实现
  "strict": true, // 开启所有严格的类型检查
  "alwaysStrict": true, // 在代码中注入'use strict'
  "noImplicitAny": true, // 不允许隐式的any类型
  "strictNullChecks": true, // 不允许把null、undefined赋值给其他类型的变量
  "strictFunctionTypes": true, // 不允许函数参数双向协变
  "strictPropertyInitialization": true, // 类的实例属性必须初始化
  "strictBindCallApply": true, // 严格的bind/call/apply检查
  "noImplicitThis": true, // 不允许this有隐式的any类型
  "noUnusedLocals": true, // 检查只声明、未使用的局部变量(只提示不报错)
  "noUnusedParameters": true, // 检查未使用的函数参数(只提示不报错)
  "noFallthroughCasesInSwitch": true, // 防止switch语句贯穿(即如果没有break语句后面不会执行)
  "noImplicitReturns": true, //每个分支都会有返回值
  "esModuleInterop": true, // 允许export=导出，由import from 导入
  "allowUmdGlobalAccess": true, // 允许在模块中全局变量的方式访问umd模块
  "moduleResolution": "node", // 模块解析策略，ts默认用node的解析策略，即相对的方式导入
  "baseUrl": "./", // 解析非相对模块的基地址，默认是当前目录
  "paths": { // 路径映射，相对于baseUrl
    // 如使用jq时不想使用默认版本，而需要手动指定版本，可进行如下配置
    "jquery": ["node_modules/jquery/dist/jquery.min.js"]
  },
  "rootDirs": ["src","out"], // 将多个目录放在一个虚拟目录下，用于运行时，即编译后引入文件的位置可能发生变化，这也设置可以虚拟src和out在同一个目录下，不用再去改变路径也不会报错
  "listEmittedFiles": true, // 打印输出文件
  "listFiles": true// 打印编译的文件(包括引用的声明文件)
}
 
// 指定一个匹配列表（属于自动指定该路径下的所有ts相关文件）
"include": [
   "src/**/*"
],
// 指定一个排除列表（include的反向操作）
 "exclude": [
   "demo.ts"
],
// 指定哪些文件使用该配置（属于手动一个个指定文件）
 "files": [
   "demo.ts"
]
```

常用配置项

```json
{
  "exclude": ["./index2.ts"], // 除了这些文件，其他的ts文件都要编译
  "include": ["./index.ts"], // 只编译这个ts文件，其他的不编译，不写默认编译全部ts文件
  "compilerOptions": {
    "target": "es2016", // 指定编译js的版本，如es5,es6
    "module": "commonjs", // 默认commonjs引入文件，可以改成es6等
    "skipLibCheck": true,
    "allowJs": true, // 是否允许编译js文件，默认不允许
    "outDir": "./dist", // ts和js编译后文件存放的目录
    "sourceMap": true, // 打包后会产生map文件(代码源文件)，会记录行数，便于寻找代码位置
    "strict": true, // 开启严格模式，比如这个模式下null不可以赋给void类型
  }
}
```

# 16 namespace命名空间

用于解决全局变量造成的污染，相当于一个封闭模块，里面的属性可以选择暴露不暴露

TypeScript与ECMAScript 2015一样，任何包含顶级import或者export的文件都被当成一个模块。相反地，如果一个文件不带有顶级的import或者export声明，那么它的内容被视为全局可见的（因此对模块也是可见的）

命名空间中通过export将想要暴露的部分导出
如果不用export 导出是无法读取其值的

```typescript
namespace a {
    export const Time: number = 1000
    export const fn = <T>(arg: T): T => {
        return arg
    }
    fn(Time)
}
 
 
namespace b {
     export const Time: number = 1000
     export const fn = <T>(arg: T): T => {
        return arg
    }
    fn(Time)
}
// 读取格式
a.Time
b.Time
```

嵌套命名空间

```typescript
namespace a {
    export namespace b {
        export class Vue {
            parameters: string
            constructor(parameters: string) {
                this.parameters = parameters
            }
        }
    }
}
 
let v = a.b.Vue
 
new v('1')
```

抽离命名空间

a.ts

```typescript
export namespace V {
    export const a = 1
}
```

b.ts

```typescript
import {V} from '../observer/index'
 
console.log(V); //{a:1}
```

简化命名空间

```typescript
namespace A  {
    export namespace B {
        export const C = 1
    }
}
 
import X = A.B.C
 
console.log(X); // 1
```

重名的命名空间会合并

避免使用`ts-node`运行带有命名空间的文件

# 17 三斜线指令

三斜线指令是包含单个XML标签的单行注释。 注释的内容会做为编译器指令使用。

三斜线指令仅可放在包含它的文件的最顶端。 一个三斜线指令的前面只能出现单行或多行注释，这包括其它的三斜线指令。 如果它们出现在一个语句或声明之后，那么它们会被当做普通的单行注释，并且不具有特殊的涵义。

`/// <reference path="..." />`指令是三斜线指令中最常见的一种。 它用于声明文件间的 依赖。

三斜线引用告诉编译器在编译过程中要引入的额外的文件。

你也可以把它理解能import，它可以告诉编译器在编译过程中要引入的额外的文件

示例

index2.ts

```typescript
namespace A  {
    export const b = 5
}
```

index3.ts

```typescript
namespace A  {
    export const a = 1
}
```

index.ts

```typescript
/// <reference path="index2.ts" />
/// <reference path="index3.ts" />

namespace A {
    export const c = 666
}
console.log(A);
```

将文件编译到lib文件夹，开启`tsconfig.json`里的`"outFile": "./lib/index.js",`

```javascript
"use strict";
var A;
(function (A) {
    A.a = 1;
})(A || (A = {}));
var A;
(function (A) {
    A.b = 5;
})(A || (A = {}));
///<reference path="index2.ts" />
///<reference path="index3.ts" />
var A;
(function (A) {
    A.c = 666;
})(A || (A = {}));
console.log(A); // { a: 1, b: 5, c: 666 }
```

## 声明文件引入

例如，把`/// <reference types="node" />`引入到声明文件，表明这个文件使用了 @types/node/index.d.ts里面声明的名字； 并且，这个包需要在编译阶段与声明文件一起被包含进来。

仅当在你需要写一个d.ts文件时才使用这个指令。

装一下node的声明文件，比如说node的声明文件

`npm install @types/node -D`

# 18 声明文件d.ts

当使用第三方库时，我们需要引用它的声明文件，才能获得对应的代码补全、接口提示等功能。

```typeScript
declare var 声明全局变量
declare function 声明全局方法
declare class 声明全局类
declare enum 声明全局枚举类型
declare namespace 声明（含有子属性的）全局对象
interface 和 type 声明全局类型
/// <reference /> 三斜线指令
```

创建`package.json`，通过`npm init -y`

装一下express，`npm install express -S`

装一下axios，`npm install axios -S`

引入axios时正常，引入express时报错，提示没有找到express的声明文件

因为在axios的package.json里已经指定了它的声明文件`"types": "index.d.ts",`，而express的package.json里没有指定。

根据提示：尝试使用 `npm i --save-dev @types/express` (如果存在)，或者添加一个包含 `declare module 'express';` 的新声明(.d.ts)文件

在这里选择第一种就好

@types就是做声明文件的，因为axios自己做了，就不用再装了，而express没做

关于这些第三发的声明文件包都收录到了 [npm](https://www.npmjs.com/~types?activeTab=packages)

目前有9千多个包

# 19 Mixins混入

可以看作合并

## 对象混入

```typeScript
interface Name {
    name: string
}
interface Age {
    age: number
}
interface Sex {
    sex: number
}

let people1: Name = { name: "小满" }
let people2: Age = { age: 20 }
let people3: Sex = { sex: 1 }

const people = Object.assign(people1, people2, people3)

console.log(people); // { name: '小满', age: 20, sex: 1 }

```

## 类的混入

严格模式要关闭不然编译不过

```typeScript
class A {
    type: boolean
    changeType(): void {
        this.type = !this.type
    }
}
// 类会将自己的属性方法挂在到自己的原型上
class B {
    name: string
    getName(): string {
        return this.name
    }
}
// 实现类
class C implements A, B {
    type: boolean = false;
    name: string = "小满";
    changeType: () => void
    getName: () => string
}
// 帮助函数
function mixins(currentClass: any, itemClass: any[]) {
    itemClass.forEach(item => {
        // 从原型上获取类的属性名
        Object.getOwnPropertyNames(item.prototype).forEach(name => {
            // 将具体的实现方法赋到当前类
            currentClass.prototype[name] = item.prototype[name]
        })
    })
}

mixins(C, [A, B])

let ccc = new C()
console.log(ccc.type); // false
ccc.changeType()
console.log(ccc.type); // true
```

# 20 装饰器Decorator

Decorator 装饰器是一项实验性特性，在未来的版本中可能会发生改变
它们不仅增加了代码的可读性，清晰地表达了意图，而且提供一种方便的手段，增加或修改类的功能

若要启用实验性的装饰器特性，你必须在命令行或tsconfig.json里启用编译器选项

`"experimentalDecorators": true`

装饰器是一种特殊类型的声明，它能够被附加到类声明，方法， 访问符，属性或参数上。

```typeScript
// 定义一个类装饰器函数 他会把ClassA的原型对象传入你的watcher函数当做第一个参数
const watcher:ClassDecorator = (target:Function)=>{
    console.log(target); // [class A]
    // 给这个类加个方法
    target.prototype.getName = <T>(name:T):T =>{
        return name
    }
}

// 这么使用，会给watcher函数回传原型对象
@watcher
class A {}

let a = new A();
// 不加断言找不到getName方法
(<any>a).getName('eeeeee');
```

## 接收参数

```typeScript
// 定义一个类装饰器函数 他会把ClassA的构造函数传入你的watcher函数当做第一个参数
const watcher = (name:string): ClassDecorator => {
    return (target: Function) => {
        target.prototype.getNames=()=>{
            return name
        }
    }
}

@watcher('小满')
class A { }

let a = new A();
console.log((<any>a).getNames()); // 小满
```

## 组合式装饰器

```typeScript
const watcher = (name: string): ClassDecorator => {
    return (target: Function) => {
        target.prototype.getNames = () => {
            return name
        }
    }
}

const log: ClassDecorator = (target: Function) => {
    target.prototype.a = 123
}

@watcher('小满')
@log
class A { }

let a = new A();
console.log((<any>a).getNames()); // 小满
console.log((<any>a).a); // 123

```

## 属性装饰器

返回两个参数

参数1：对于静态成员来说是类的构造函数，对于实例成员是类的原型对象。

参数2：成员的名字。

```typeScript
const log:PropertyDecorator = (...arg) => {
    console.log(arg); // [ {}, 'name', undefined ]
}

class A {
    @log
    name:string
}
```

## 属性、方法装饰器

返回三个参数

参数1：对于静态成员来说是类的构造函数，对于实例成员是类的原型对象。

参数2：成员的名字。

参数3：成员的属性描述符。

```typeScript
const log:MethodDecorator = (...arg) => {
    console.log(arg);
}

class A {
    name:string
    @log
    getName(){
        return '7878'
    }
}

```

```typeScript
[
  {},
  'getName',
  {
    value: [Function: getName],
    writable: true,
    enumerable: false,
    configurable: true
  }
]
```

## 参数装饰器

参数1：对于静态成员来说是类的构造函数，对于实例成员是类的原型对象。

参数2：成员的名字。

参数3：参数的索引

```typeScript
const log: ParameterDecorator = (...arg) => {
    console.log(arg); // [ {}, 'getName', 0 ]
}

class A {
    name: string
    getName(@log name: string) {
        return '7878'
    }
}
```

```typeScript
const log: ParameterDecorator = (...arg) => {
    console.log(arg); // [ {}, 'getName', 1 ]
}

class A {
    name: string
    getName(name: string, @log age: number) {
        return '7878'
    }
}
```

# 21 Rollup构建TS项目 & webpack构建TS项目

## Rollup

rollup打包的体积较小，用来做一些小项目是完全没问题的

`npm init -y`生成package.json

新建`rollup.config.js`rollup的配置文件

新建`src`、`public`文件夹

新建`src/index.ts`、`public/index.html`

`tsc --init`生成ts的配置文件

安装依赖
1.全局安装rollup `npm install rollup -g`

2.安装TypeScript   `npm install typescript -D`

3.安装TypeScript 转换器 `npm install rollup-plugin-typescript2 -D`

4安装代码压缩插件 `npm install rollup-plugin-terser -D`

5 安装rollupweb服务 `npm install rollup-plugin-serve -D`

6 安装热更新 `npm install rollup-plugin-livereload -D`

7引入外部依赖 `npm install rollup-plugin-node-resolve -D`

8安装配置环境变量用来区分本地和生产  `npm install cross-env -D`

9替换环境变量给浏览器使用 `npm install rollup-plugin-replace -D`

package.json

```json
{
  "name": "rollupTs",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    // -w是--watch，就是监听，页面有改动会,cross-env NODE_ENV=development是环境变量
    // 起服务时，给node环境变量赋值development
    "dev": "cross-env NODE_ENV=development  rollup -c -w",
    // -c是读取当前目录下的rollup.config.js
    // 打包时给node环境变量赋值production，区分是生产还是开发环境
    "build":"cross-env NODE_ENV=production  rollup -c"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "cross-env": "^7.0.3",
    "rollup-plugin-livereload": "^2.0.5",
    "rollup-plugin-node-resolve": "^5.2.0",
    "rollup-plugin-replace": "^2.2.0",
    "rollup-plugin-serve": "^1.1.0",
    "rollup-plugin-terser": "^7.0.2",
    "rollup-plugin-typescript2": "^0.31.1",
    "typescript": "^4.5.5"
  }
}
```

tsconfig.json里的mudule改成ES2015

rollup.config.js

```javascript
// 查看环境变量，可以把"dev"里的-w删了再看，但浏览器里看不了，这是弄得里的
// console.log(process.env);
import path from 'path'
import ts from 'rollup-plugin-typescript2'
import serve from 'rollup-plugin-serve'
import livereload from 'rollup-plugin-livereload'
import { terser } from 'rollup-plugin-terser'
import replce from 'rollup-plugin-replace'
const isDev = () => {
    return process.env.NODE_ENV === 'development'
}
export default {
    input: "./src/index.ts", // 入口
    // 出口
    output: {
        // 编译输出出口文件
        file: path.resolve(__dirname, './lib/index.js'),
        // 编译格式
        format: "umd",
        // 开启suorcemap
        sourcemap: true
    },
    plugins: [
        // 会去读取tsconfig.json
        ts(),
        // 这个是做热更新的
        isDev() && livereload(),
        // 代码压缩，给代码压缩到一行
        terser({
            compress: {
                // 可以删掉console.log
                drop_console: !isDev()
            }
        }
        ),
        // 把环境变量注册到浏览器
        replce({
            'process.env.NODE_ENV': JSON.stringify(process.env.NODE_ENV),
            'a': 'sss', // 自定义的
        }),
        // 这个插件用来起前端服务
        isDev() && serve({
            open: true, // 是否打开页面
            port: 1998, // 指定端口
            openPage: "/public/index.html", // 指定打开页面
        })
    ]
}
```

`npm run build`打包后会生成lib文件

在public/index.html里引入

`npm run dev`rollup服务就起好了

index.ts

```typeScript
const a: string = "小满1"

if (process.env.NODE_ENV === 'development') {
    alert('开发')
} else {
    alert('生产')
}
```

index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <script src="../lib/index.js"></script>
</body>
</html>
```

## webpack

`npm init -y`生成package.json

新建`src`、`public`文件夹

新建`src/index.ts`、`public/index.html`

`tsc --init`生成ts的配置文件

安装webpack   `npm install webpack -D`

webpack4以上需要 `npm install  webpack-cli -D`

编译TS  `npm install ts-loader -D`

TS环境 `npm install typescript -D`

热更新服务 `npm install webpack-dev-server -D`

HTML模板 `npm install html-webpack-plugin -D`

新建webpck.config.js

```typeScript
const path = require('path')
const htmlwebpackplugin = require('html-webpack-plugin')

module.exports = {
    entry: "./src/index.ts",
    mode: "development",
    output: {
        path: path.resolve(__dirname, './dist'),
        filename: "index.js"
    },
    module: {
        rules: [
            {
                test: /\.ts$/, // 以.ts结尾的文件
                use: "ts-loader", // 指定编译器
            }
        ]
    },
    // 匹配后缀
    resolve: {
        extensions: ['.js', 'ts'], // import时就不用写后缀了
        alias: {
            '@': path.resolve(__dirname, './src')
        }
    },
    // 开发环境的服务
    devServer: {
        port: 1998,
        // 跨域代理
        proxy: {}
    },
    plugins: [
        new htmlwebpackplugin({
            template: './public/index.html' // 指定模板
        })
    ]
}
```

package.json

```json
{
  "name": "webpackts",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "webpack-dev-server",
    "build": "webpack"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "html-webpack-plugin": "^5.5.0",
    "ts-loader": "^9.3.0",
    "typescript": "^4.7.3",
    "webpack": "^5.73.0",
    "webpack-cli": "^4.10.0",
    "webpack-dev-server": "^4.9.2"
  }
}
```

# 22 TS编写发布订阅模式

