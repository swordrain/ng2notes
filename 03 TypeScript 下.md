# 模块

## 模块导出

导出声明
```
export const COMPANY = "GF"; //导出变量

export interface IdentityValidate { //导出接口
    isGfStaff(s: string): boolean;
}

export class ErpIdentityValidate implements IdentityValidate { //导出类
    isGfStaff(s: string) {
        return erpService.contains(erp); 
    }
}
```

导出语句（重命名）
```
class ErpIdentityValidate implements IdentityValidate { //导出类
    isGfStaff(s: string) {
        return erpService.contains(erp); 
    }
}

export { ErpIdentityValidate };
export { ErpIdentityValidate as GFIdentityValidate };
```

模块包装
```
export { ErpIdentityValidate as RegExpBasedZipCodeValidator } from "./ErpIdentityValiate";

export * from "./IdentityValidate";
```

## 模块导入
```
//导入一个模块
import { ErpIdentityValidate } from "./ErpIdentityValidate";
let erp = new ErpIdentityValidate();

//别名导入
import { ErpIdentityValidate as ERP } from "./ErpIdentityValidate";
let erp = new ERP();

//全体导入
import * as validator from "./ErpIdentityValidate";
let erp = new validator.ErpIdentityValidate();
```

## 模块的默认导出
加上关键字default，每个模块可以有一个默认导出。类和函数声明可以省略导出名来实现默认导出。

默认导出类
```
//ErpIdentityValidate.ts
export default class ErpIdentityValidate implements IdentityValidate {
    isGfStaff(s: string) {
        return erpService.contains(erp); 
    }
}
//test.ts
import validator from './ErpIdentityValidate';
let erp = new Validator();
```
默认导出函数
```
//nameServiceValidate.ts
export default function(s: string){ //注意没函数名
    return nameService.contains(s);
}
//test.ts
import validator from "./ErpIdentityValidate";
validator('Hello World");
```
默认导出值
```
//constantService.ts
export default "Angular";
//test.ts
import name from "./constantService";
console.log(name);
```

# 接口
## 属性类型接口
```
interface FullName {
    firstName: string,
    secondName: string;
}

function printLabel(name: FullName) {
    console.log(name.firstName + " " + name.secondName);
}

let myObj = { age: 10, firstName: "Jim", secondName: "Raynor" };
printLabel(myObj);
```
只要形式上满足接口的要求即可，比如含有firstName和secondName属性  
也可以声明一个可选的属性，可选属性不一定要传
```
interface FullName {
    firstName: string,
    secondName?: string;
}

function printLabel(name: FullName) {
    console.log(name.firstName + " " + name.secondName);
}

let myObj = { age: 10, firstName: "Jim"};
printLabel(myObj);
```

## 函数类型接口
```
interface encrypt {
    (val: string, salt: string): string
}

let md5: encrypt;
md5 = function(val: string, salt: string) {
    console.log("origin value: " + val);
    let encryptValue = doMd5(val, salt);
    console.log("encrypt value: " + encryptValue);
    return encryptValue;
}
let pwd = md5("password", "Angular");
```
参数个数要相同，参数类型一致，返回值类型一致，参数名可以不一样

## 可索引类型接口
```
interface UserArray {
    [index: number]: string;
}
interface UserObject {
    [index: string]: string;
}

let userArray: UserArray = ["张三", "李四"];
let userObject: UserObject = {"name", "张三"};

console.log(userArray[0]);
console.log(userObject["name"]);
```

## 类类型接口
```
interface Animal {
    name: string;
    setName(n: string): void;
}
class Dog implements Animal {
    name: string;
    setName(n: string) {
        this.name = n;
    }
    constructor(n: string) { }
}
```

## 接口扩展
```
interface Person extends Animal {
    talk(): void;
}
```

# 装饰器
## 方法装饰器
```
declare type MethodDecorator = <T>(target: Object, propertyKey: string | symbol, descriptor: TypedPropertyDescriptor<T> =>
    TypedPropertyDescriptor<T> | void;
    //target: 类的原型对象
    //propertyKey: 方法的名字
    //descriptor: 成员属性描述
```
一个实例
```
class TestClass {
    @log
    testMethod(arg: string) {
        return "logMsg: " + arg;
    }
}

function log(target: Object, propertyKey: string, descriptor: TypedPropertyDescriptor<any>) {
    let origin = descriptor.value;
    descriptor.value = function(...args: any[]) {
        console.log("args: " + JSON.stringify(args)); //调用前
        let result = origin.apply(this, args); //调用方法
        console.log("The result - " + result); //调用后
        return result;
    };
    return descriptor;
}
```
## 类装饰器
```
@Component({
    selector: 'person',
    templateUrl: 'person.html'
})
class Person {
    constructor(
        public firstName: string,
        public secondName: string
    ){ }
}
```
## 参数装饰器
```
class userService {
    login(@inject name: string) { }
}
```
## 属性装饰器

## 装饰器组合
可以叠加多个装饰器，从左到右从上到下执行

# 泛型
>也跟后端语言一样
```
class MinHeap<T> {
    list: T[] = [];
    add(element: T): void {
    
    }
    min(): T {
    
    }
}
let heap1 = new MinHeap<number>();
```

# ts周边
## 编译配置文件
tsconfig.json文件
```
{
    "compilerOptions": {
        "target": "es5",
        "module": "commonjs",
        "declaration": false,
        "noImplicityAny": false,
        "removeComments": true,
        "noLib": false,
        "emitDecoratorMetadata": true,
        "experimentalDecorators": true,
        "sourceMap": true
    },
    "exclude": [
        "node_modules",
        "typings/browser.d.ts",
        "typings/browser/**"
    ],
    "compileOnSave": false
}
```
## 声明文件
如果要识别第三方库的类型，需要使用声明文件。声明文件以.d.ts为后缀，描述了一个JavaScript模块文件所有导出的接口类型信息。

如lodash
```
npm install --save @types/loadsh
```

```
import * as _ from "loadsh";
_.padStart("Hello Angular!", 2, " ");
```
在tsconfig.json文件中可以使用typeRoots来指定允许查找的包的文件夹路径，
```
{
    "compilerOptions": {
        "typeRoots": ["./typings"]
    }
}
```
上面的配置表示编译器会去查找所有"./typings"目录下的包，但不包含./node_moduels/@types里的包  
如果在tsconfig.json中配置了types，那么只有配置的包才会被包含进来。
```
{
    "compilerOptions": {
        "types": ["loadsh", "koa"]
    }
}
```
