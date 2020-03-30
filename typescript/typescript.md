# typescript

[TOC]

## 1. 基础类型

* bool

* number

* string

* array

* tuple（元组）

  ```typescript
  let x:[string, number];
  x = ["hello", 10];
  
  // 当索引越界赋值时，会使用联合类型
  x[3] = "world";	// 可以，可以赋值给(string | number)类型
  ```

* enum(枚举)

  ```typescript
  enum Color {Red, Green, Blue}	// 默认情况，编号从0开始
  enum Color {Red = 1, Green, Blue}	// 从1开始编号
  
  console.log(Color[1])	//输出"Red"
  ```

* any(任意值)

* void(空值)

  可以赋值**undefined**和**null**

* never(无返回值)

  可用于检测报错或者死循环

## 2. 类型断言

```typescript
// 方法1
let someValue: any = "a";
let len: number = (<string>someValue).length;	// 尖括号方法 此处要断言，因为是any类型

// 方法2
let value: any = "a";
let len: number = (value as string).length	// as方法（jsx只能用as）
```

## 3. 变量声明

* let 

* var

* const

* 解构赋值

  ```typescript
  let [a, b]:[number, number] = [1, 2];
  let {a, b}:{a: number, b: number} = {a: 1, b: 2};
  ```

* 函数声明

  ```typescript
  type C = {a: string, b?: number}
  function f({a, b}: C):void{
      
  }
  ```

## 4. 接口

* 普通接口

```typescript
interface LabelledValue {
    label: string;
}

// 只能接收一个只带有label属性的对象
function printLabel(labelledObj: LabelledValue){
    console.log(labelledObj.label)
}
```

* 可选属性

```typescript
interface SquareConfig {
    color?: string;
    width?: number;
}
```

* 只读属性

```typescript
interface Point {
    readonly x: number;
    readonly y: number;
}
let p1: Point = {x: 1, y: 1}	// 无法再修改
```

​	有ReadonlyArray<T>类型

```typescript
let r0: ReadonlyArray<number> = [1, 2]
r0[0] = 2; // error
```

* readonly和const

  如果作为属性，使用readonly

  如果作为变量，使用const

* 绕开额外检查

  1、使用类型断言

  2、添加索引签名

```typescript
interface SquareConfig {
    color?: string;
    width?: number;
    [propName: string]: any;
}
```

* 函数类型

```typescript
interface SearchFunc {
    (source: string, subString: string): boolean
}
let mySearch: SearchFunc;

// 参数名称不需要对应，只需要位置对应即可
mySearch = function(source1: string, subString1: string): boolean {
    return true
}

// 也可以根据类型推断来书写
mySearch = function(src, sub) {
    let result = src.search(sub);
    return result > -1;
}
```

* 可索引的类型

```typescript
interface StringArray {
   [index: number]: string;
}
  
let myArray: StringArray;
myArray = {0:"Bob", 1:"Fred"};	// 只要键为num， 值为string即可

let myStr: string = myArray[0];

interface NumberDictionary {
  [index: string]: number;
  length: number;    // 可以，length是number类型
  name: string       // 错误，`name`的类型与索引类型返回值的类型不匹配
}
```

* 类类型

  实现接口

```typescript
interface ClockInterface {
    currentTime: Date;
}

class Clock implements ClockInterface {
    currentTime: Date;
    constructor(h: number, m: number) { }
}
let obj: Clock;
obj = new Clock(1, 2)
console.log(obj.currentTime = new Date)
```

​		类静态部分与实例部分的区别

```typescript
interface ClockConstructor {
    new (hour: number, minute: number): ClockInterface;
}
interface ClockInterface {
    tick();
}

// 需要传入带new方法的函数，返回一个实例
function createClock(ctor: ClockConstructor, hour: number, minute: number): ClockInterface {
    return new ctor(hour, minute);
}

class DigitalClock implements ClockInterface {
    constructor(h: number, m: number) { }
    tick() {
        console.log("beep beep");
    }
}
class AnalogClock implements ClockInterface {
    constructor(h: number, m: number) { }
    tick() {
        console.log("tick tock");
    }
}

let digital = createClock(DigitalClock, 12, 17);
let analog = createClock(AnalogClock, 7, 32);
```

	* 继承接口

```typescript
interface Shape {
    color: string;
}

interface PenStroke {
    penWidth: number;
}

interface Square extends Shape, PenStroke {
    sideLength: number;
}

let square = <Square>{};
square.color = "blue";
square.sideLength = 10;
square.penWidth = 5.0;
```

	* 混合类型

```typescript
interface Counter {
    (start: number): string;
    interval: number;
    reset(): void;
}

function getCounter(): Counter {
    let counter = <Counter>function (start: number) { };
    counter.interval = 123;
    counter.reset = function () { };
    return counter;
}

let c = getCounter();
c(10);
c.reset();
c.interval = 5.0;
```

 * 接口继承类

   **private**只有本类中可以访问，但是可以继承

   **protected**可以在派生类中访问并继承

```typescript
class Control {
    private state: any;
}

interface SelectableControl extends Control {
    select(): void;
}

class Button extends Control implements SelectableControl {
    select() { }
}

class TextBox extends Control {

}

// Error: Property 'state' is missing in type 'Image'.
class Image implements SelectableControl {
    select() { }
}

class Location {

}
```

## 5.类

* 继承

```typescript
class Animal {
    move(distanceInMeters: number = 0) {
        console.log(`Animal moved ${distanceInMeters}m.`);
    }
}

class Dog extends Animal {
    bark() {
        console.log('Woof! Woof!');
    }
}

const dog = new Dog();
dog.bark();
dog.move(10);
dog.bark();
```

* public、private、protected

  public（默认）：是访问的最高权限，可以在类的外部访问

  private：不能在它的类的外部访问，可以继承，但是无法通过子类访问

  protected：与private相似，但在派生类中的成员仍然可以访问

```typescript
class Animal {
    constructor(private name: string) { }
    move(distanceInMeters: number) {
        console.log(`${this.name} moved ${distanceInMeters}m.`);
    }
}
```

* readonly（属性设置为只读，必须在声明时或构造函数里被初始化）

* 存取器

  getter、setter

```typescript
let passcode = "secret passcode";

class Employee {
    private _fullName: string;

    get fullName(): string {
        return this._fullName;
    }

    set fullName(newName: string) {
        if (passcode && passcode == "secret passcode") {
            this._fullName = newName;
        }
        else {
            console.log("Error: Unauthorized update of employee!");
        }
    }
}
```

* 静态属性

  static

* 抽象类

  作为其他派生类的基类使用，一般不会直接被实例化，**abstract**用于定义抽象类和在抽象类内部定义抽象方法

```typescript
abstract class Animal {
    abstract makeSound(): void;	// 必须在派生类中实现
    move(): void {
        console.log('roaming the earch...');
    }
}

class Dog extends Animal{
    makeSound(){
        console.log("汪汪汪")
    }
}
```

	* 当作构造函数使用
	* 当作接口使用

```typescript
class Point {
    y: number;
    x: number;
}
interface Point3d extend Point {
    z: number;
}
```

## 6. 函数

	* 完整的书写方式

```typescript
let myAdd: (x: number, y: number) => number =
    function(x: number, y: number): number {
        return x + y;
    }
```

 * 可选参数和默认参数

   可选参数必须跟在必须参数的后面

```typescript
function buildName(firstName: string, lastName?: string) {
    if (lastName)
        return firstName + " " + lastName;
    else
        return firstName;
}
```

	* 剩余参数

```typescript
function buildName(firstName: string, ...restOfName: string[]) {
  return firstName + " " + restOfName.join(" ");
}

let employeeName = buildName("Joseph", "Samuel", "Lucas", "MacKinzie");
```

 * this

   this参数是一个假参数，出现在参数列表的最前面

   如果给编译器设置**--noImplicitiThis**，它会指出this类型为any

```typescript
interface Card {
    suit: string;
    card: number;
}
interface Deck {
    suits: string[];
    cards: number[];
    createCardPicker(this: Deck): () => Card;
}
let deck: Deck = {
    suits: ["hearts", "spades", "clubs", "diamonds"],
    cards: Array(52),
    // NOTE: The function now explicitly specifies that its callee must be of type Deck
    createCardPicker: function() {
        return () => {
            let pickedCard = Math.floor(Math.random() * 52);
            let pickedSuit = Math.floor(pickedCard / 13);
            // 此处的this显示Deck
            return {suit: this.suits[pickedSuit], card: pickedCard % 13};
        }
    }
}
```

 * 重载

   根据传入参数的不同而返回不同的结果

```typescript
let suits = ["hearts", "spades", "clubs", "diamonds"];

// 此处只有两个重载列表，一个是接收对象另一个接收数字
function pickCard(x: {suit: string; card: number; }[]): number;
function pickCard(x: number): {suit: string; card: number; };
function pickCard(x: any): any {
    // Check to see if we're working with an object/array
    // if so, they gave us the deck and we'll pick the card
    if (typeof x == "object") {
        let pickedCard = Math.floor(Math.random() * x.length);
        return pickedCard;
    }
    // Otherwise just let them pick the card
    else if (typeof x == "number") {
        let pickedSuit = Math.floor(x / 13);
        return { suit: suits[pickedSuit], card: x % 13 };
    }
}

let myDeck = [{ suit: "diamonds", card: 2 }, { suit: "spades", card: 10 }, { suit: "hearts", card: 4 }];
let pickedCard1 = myDeck[pickCard(myDeck)];
alert("card: " + pickedCard1.card + " of " + pickedCard1.suit);

let pickedCard2 = pickCard(15);
alert("card: " + pickedCard2.card + " of " + pickedCard2.suit);
```

## 7. 泛型

```typescript
function identity<T>(arg: T): T {
    return arg;
}

let output = identity<string>("hello");
```

​	在反省中必须把参数当作是任意类型

```typescript
function identity<T>(arg: T): T {
    return arg;
}

function loggingIdentity<T>(arg: T): T {
    console.log(arg.length);  // 此时还不知道arg是否存在length属性
    return arg;
}
```

​	解决方法

```typescript
function loggingIdentity<T>(arg: Array<T>): Array<T> {
    console.log(arg.length);  // OK
    return arg;
}
```

	* 泛型接口

```typescript
interface Generi<T> {
    (arg: T): T;
}

let myGeneri: Generi<string> = function <T>(s: T): T{
    return s;
}
```

	* 泛型类

```typescript
class GenericNumber<T> {
    zeroValue: T;
    add: (x: T, y: T) => T;
}

let myGenericNumber = new GenericNumber<number>();
myGenericNumber.zeroValue = 0;
myGenericNumber.add = function(x, y) { return x + y; };	// 此处x，y只能是number
```

 * 泛型约束

   使用一个接口来约束泛型，使用**extend**关键字

```typescript
interface Lengthwise {
    length: number;
}

// 此时传入的参数必须具有length属性
function logginIdentity<T extends Lengthwise>(arg: T): T {
    console.log(arg.length);
    return arg;
}
```

 * 在泛型约束中使用类型参数

   例如我们呢想要用属性名从对象里获取这个属性，并且要确保这个属性存在于对象obj上，因此我们在两个类型之间使用约束

```typescript
function getProperty<T, K extends keyof T>(obj: T, key: K) {
    return obj[key];
}

let x = { a: 1, b: 2, c: 3, d: 4 };

getProperty(x, "a"); // OK
getProperty(x, "m"); // x中没有m属性
```

 * 在泛型里使用类类型

   使用泛型创建工厂函数，需要引用构造函数的类类型

```typescript
function create<T>(c: {new(): T;}): T {
    return new c();
}
class createObj{}
create(createObj)

class BeeKeeper {
    hasMask: boolean;
}
class ZooKeeper {
    nametag: string;
}
class Animal {
    numLegs: number;
}
class Bee extends Animal {
    keeper: BeeKeeper;
}
class Lion extends Animal {
    keeper: ZooKeeper;
}
function createInstance<A extends Animal>(c: new () => A): A {
    return new c();
}

createInstance(Lion).keeper.nametag;  // OK!
createInstance(Bee).keeper.hasMask;   // OK!
```

