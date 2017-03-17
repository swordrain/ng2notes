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
export {ErpIdentityValidate as RegExpBasedZipCodeValidator } from "./ErpIdentityValiate";

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
```
//全体导入
import * as validator from "./ErpIdentityValidate";
let erp = new validator.ErpIdentityValidate();
```

## 模块的默认导出
