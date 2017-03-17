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
