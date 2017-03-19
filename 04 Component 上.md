# ng2的组件
等同于Web Component或者React里的组件  
同时还包含了ng自己的一些概念，如服务等

# 组件基础
1. 从@angular/core中导入Component装饰器 Component Decorator
2. 建立普通类，用@Component修饰
3. 在@Component中，设置selector和template

一个简单的例子
```
import { Component } from '@angular/core';
@Component({
            selector: 'contact-item',
            template: `
                <div>
                    <p>张三</p>
                    <p>13800000000</p>
                </div>
                `
         })
export class ContactItemComponent { }
```
在父组件中使用`<contact-item></contact-item>`  
装饰器里`selector` `template`等内容称为组件元数据 Component Metadata   
`template`或`templateUrl`定义的内容称为模板 template
定义的class称为组件类，包含逻辑实现
>似乎等同于ng1的Controller

### 组件元数据  
*selector*  
string  
用烤肉串方式命名，如`hello-world`

*template*  
string  
建议使用`语法包含多行字符串  

*templateUrl*  
string  
外部模板的URL地址，
```
@Component({
           templateUrl: 'app/components/contact-item.html'
           })
```

*styles styleUrls*  
string[]  
注意是数组形式，如果同时指定，styles中的样式先被解析，意味着被styleUrls覆盖

*inputs outputs*  
stirng[]  
指定组件的输入输出属性

*host*  
{[key: string]: string;}
指定指令/组件的事件、动作和属性等

*providers*
any[]
指定该组件及其所有子组件（含ContentChildren）可用的服务

*exportAs*  
string  
给指令分配一个变量，使得可以在模板中调用

*moduleId*  
string  
包含该组件模块的id，被用于解析模板和样式的相对路径

*queries*  
{[key: string]:any;}  
设置需要被注入到组件的查询  

*viewProviders*  
any[]  
指定该组件及其所有子组件（不含ContentChildren）可用的服务

*changeDetection*  
ChangeDetectionStrategy  
指定使用的变化监测策略

*animations*  
AnimationEntryMetadata[]  
设置Angular动画

*encapsulation*
ViewEncapsulation  
设置组件的视图包装选项

*interpolation*  
[string, stirng]  
设置自定义插值标记，默认是{{}}

### 数据绑定与事件坚挺
`{{}}`用来单向绑定  
双向绑定
```
<input type="text" value="{{name}}" [(ngModel)]="name" />
```
事件
```
<i (click)="addContact()"><i>
```
其中addContact是定义在组件类里的方法  

### 模块
>概念类似ng1中的module，使用类似Android中的Manifest文件，用以声明各种组件
模块使用@NgModule修饰，一个应用可以有多个模块，但只能有一个根模块  
```
//app.module.ts
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { FormsModule } from '@angular/forms';
import { HttpModule } from '@angular/http';

import { AppComponent } from './app.component';
import { HeroDetailComponent } from './hero-detail.component';

@NgModule({
  declarations: [
    AppComponent,
    HeroDetailComponent
  ],
  exports: [],
  imports: [
    BrowserModule,
    FormsModule,
    HttpModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

*bootstrap*  
启动组件 类似Android里action Main category LAUNCH的那个Activity  

*providers*  
模块依赖的服务，能被模块中的所有组件共享同一个实例，如果在组件的providers元数据引入的服务，只在该组件及其子组件里共享同一个实例    

*imports*  
引入该模块依赖的其他模块或路由，引入后模块里的组件模板才能引用外部对应的组件、指令和管道  
>同ng1

*exports*  
导出视图类。当该模块被引入到外部模块时，这个属性指定了外部模块可以使用哪些视图类，所以它的值类型跟declarations一致（组件、指令、管道）

*declarations*  
指定属于这个模块视图类，包含组件、指令、管道。
>好比在Android Manifest文件里声明四大组件

应用的启动
```
//main.ts
import { enableProdMode } from '@angular/core';
import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';

import { AppModule } from './app/app.module';
import { environment } from './environments/environment';

if (environment.production) {
  enableProdMode();
}

platformBrowserDynamic().bootstrapModule(AppModule);
```
