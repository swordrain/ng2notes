# 编译工具
```
npm install -g typescript
```

```
tsc hello.ts
```
在当前目录下会编译出hello.js文件

# 基本类型
* boolean
* number
* string
* array
* tuple
* enum
* any
* null undefined
* void
* never

## boolean
```
let flag: boolean = true; //像swift的声明方式
flag = 1; //报错
```

## number
在ts里，数字都是浮点型，能支持二级制、八进制、十六进制字面量
```
let binaryLiteral: number = 0b1010; //二进制
let octalLiteral: number = 0o744; //八进制
let decLiteral: number = 6; //十进制
let hexLiteral: number = 0xf00d; //十六进制
```

## string
支持用`来定义多行字符串，使用${expr}的形式嵌入变量或表达式
>跟es6一样
```
let name: string = "Angular";
let years: number = 5;
let words: string = `你好，今年是${ name }发布${ years + 1 }周年`
```

## array
```
//两种定义方式
let arr: number[] = [1, 2];
let arr: Array<number> = [1, 2];
```

## tuple
>swift里也有
```
let x: [string, number];
x = ["Angular", 25]; 
x = [10, 'Angular']; //error
console.log(x[0]);
```

## enum
```
enum Color {Red, Green, Blue};
let c: Color = Color.Blue;
console.log(c);
//默认关联值是0，可以修改
enum Color {Red = 2, Blue, Green = 0}
```
>感觉把ts做的更像后端语言了

## any
>更像js里的var声明变量，可以为任意类型
* 跳过编译类型检查，可能是数据类型的检查和对象方法存在的检查
* 任意类型的数组
```
let arrayList: any[] = [1, false, "fine"];
arrayList[1] = 100;
```

## null undefined
null undefined可以赋值给其他类型，如果在ts中启用--strictNullChecks特性，可以使null和undefined只能被赋给void或本身对应的类型
```
//启用 --strictNullChecks
letx : number;
x = 1;
x = undefined; //error
x = null; //error
```
用|来支持多种类型
>像swift里的可选类型了
```
//启用 --strictNullChecks
let x: number;
let y: number | undefined;
let z: number | null | undefined;
```

## void
函数没有返回值，就等于是返回void类型
```
function hello(): void {
    alert("Hello Angular");
}
```

## never
never是其他类型的子类型，代表从来不会出现的值。never类型的变量只能被never类型所赋值，在函数中通常表现为抛出异常或无法执行到终止点（如无限循环）
```
let x: never;
let y: number;

x = 123; //error
x = (() => { throw new Error('exception occur')})(); //correct
y = (() => { throw new Error('exception occur')})(); //correct

function error(message: string): never {
    throw new Error(message);
}

function loop(): never {
    while(true){ }
}
```

# 声明和解构
>跟es6一样

# 函数
## 函数定义  
>也像swift的func
```
function maxA(x: number, y: number): number {
    return x > y ? x : y;
}
```

## 可选参数
```
function max(x: number, y?: number): number{
    if(y){
        return x > y ? x : y;
    } else {
        return x;
    }
}
```

## 默认参数
```
function max(x: number, y = 4): number {
    return x > y ? x : y;
}
```
>跟es6一样，只有传入undefined的时候，默认值才起作用，如果默认参数放在前面，就必须在第一个参数上显式的传入undefined了

## 剩余参数
```
function sum(x: nunmber, ...restOfNumber: number[]): number{
    let result = x;
    for(let i = 0; i < restOfNumber.length; i++) {
        result += resultOfNumber[i];
    }
    return result;
}
```

## 函数重载
ts中有函数重载，根据参数类型和定义顺序进行匹配，所以定义的时候把最精确的定义放在前面

## 箭头函数
>跟es6一样

# 类
## 类的例子
```
class Car {
    engine: string;
    constructor(engine: string) {
        this.engine = engine;
    }
    drive(distanceInMeters: number = 0) {
        console.log(distanceInMeters);
    }
}
let car = new Car("petrol");
car.drive(100);
```
>很像后端语言的类定义

## 继承与多态
```
class MotoCar extends Car{
    constructor(engine: string) { super(engine); } //必须调用super()
}
class Jeep extends Car{
    constructor(engine: string) { super(engine); } //必须调用super()
    drive(distanceInMeters: number = 100) {
        console.log("Jeep...");
        return super.drive(distanceInMeters);
    }
}
let tesla = new MotoCar("electricity");
let landRover: Car = new Jeep("petrol");
tesla.drive();
landRover.drive(200); //多态，调用子类的方法
```

## 修饰符
public private protected，作用跟后端语言一样

## 参数属性
```
class Car {
    constructor(protected engine: string) {

    }
    drive(distanceInMeters: number = 0) {
        console.log(distanceInMeters);
    }
}
```
构造函数的参数如果加上访问限定符，则会自动在类里创建并初始化成员变量（注意constructor里是空的）

## 静态属性
static修饰

## 抽象类
abstract，可以修饰类也可以修饰方法，也类似后端语言
