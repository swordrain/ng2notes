# 组件交互
>类似React的组件交互，Web Component应该都这么玩

## 输入输出属性
使用`@Input()`和`@Output()`修饰
```
//item.component.ts
export class ListItemComponent {
    @Input() contact: any = {};
    @Output() routerNavigate = new EventEmitter<number>();
    ...
}

//list.component.html
<list-item [contact]="contact" (routerNavigate)="routerNavigate($event)"></list-item>
```
另外也可以在元数据中使用`inputs`和`outputs`，名称要对应
```
@Component({
    ...,
    inputs: ['contact'],
    outputs: ['routerNavigate']
})
export class ListItemComponent{
    contact: any = {};
    routerNavigate = new EventEmitter<numbber>();
    ...
}
```
## 父向子传递数据
通过在父组件引用子组件处设置属性
```
<list-item [contact]="contact"></list-item>
```
其中`contact`关联到父组件类中的实例变量

**拦截输入**  
*使用setter拦截*  
```
export class ListItemComponent {
    _contact: object = {};
    @Input() 
    set contactObj(contact: object){
        this._contact.name = (contact.name && contact.name.trim()) || 'no name set';
        this._contact.telNum = contact.telNum || '000-000';
    }
    get contactObj() { return this._contact; }
```
*使用ngOnChanges监听*  
ngOnChanges是组件声明周期钩子之一，当数据绑定的输入属性的值发生变化时被触发  
```
@Component({
    selector: 'change-log',
    template: `
        <h4>Change Log: </h4>
        <ul>
            <li *ngFor="let change of changes">{{change}}</li>
        </ul>
        `
    })
export class ChangeLogComponent implements OnChanges {
    @Input() contact: any = {};
    changes: string[] = [];
    ngOnChanges(changes: {[propKey: string]: SimpleChanges }) {
        let log: string[] = [];
        for (let propName in changes) {
            let changedProp = changes[propName],
                from = JSON.stringify(changedProp.previousValue),
                to = JSON.stringify(changedProp.currentValue);
            log.push(`${propName}` changed from ${from} to ${to}`);
        }
        this.changes.push(log.join(','));
    }
}
```
## 子向父传递数据
父组件
```
@Component({
    selector: 'collection',
    template: `
        <contact-collect [contact]="detail" (onCollect)="collectTheContact($event)"></contact-collect>
    `
})
export class CollectionComponent {
    detail: any = {};
    collectTheContact() {
        this.detail.collection == 0 ? this.detail.collection = 1 : this.detail.collection = 0;
    }
}
```
子组件
```
@Component({
    selector: 'contact-collect',
    template: `
        <i [ngClass]="{collected: contact.collection}" (click)="collectTheContact()">收藏</i>
    `
})
export class ContactCollectionComponent {
    @Input() contact: any = {};
    @Output() onCollect = new EventEmitter<boolean>();
    collectTheContact() {
        this.onCollect.emit();
    }
}
```
## 其他组件交互方式

**通过局部变量**
```
@Component({
    selector: 'collection',
    template: `
        <contact-collect (onCollect)="collect.collectTheContact($event)" #collect></contact-collect>
    `
})
```
collect就是子组件实例的引用
**使用@ViewChild**
如果参数为类实例，表示父组件将绑定一个指令或子组件实例
如果参数为字符串，表示绑定字符串作为选择器所选择的模板内容
```
//修改父组件
@Component({
    selector: 'collection',
    template: `
        <contact-collect (onCollect)="collectTheContact()"></contact-collect>
    `
})
export class CollectionComponent {
    @ViewChild(ContactCollectCimponent) contactCollect: ContactCollectComponent;
    collectTheContact() {
        this.contactCollect.collectTheContact();
    }
}
```
# 组件内容嵌入
>相当于React的props.children
使用`<ng-content>`，`select`用于匹配一个内容的部分
```
@Component({
    selector: 'example-content',
    template: `
        <div>
            <div>
                <ng-content select="header"></ng-content>
            </div>
            <div>
                <ng-content select=".class-select"></ng-content>
            </div>
            <div>
                <ng-content select="[name=footer]"></ng-content>
            </div>
        </div>
        `
    })
```
使用的时候
```
@Component({
    selector: 'app',
    template: `
        <example-content>
            <header>Header content</header>
            <div class="class-select">
                div with .class-select
            </div>
            <div name="footer">Footer content</div>
        </example-content>
    `
})
```

# 组件生命周期
在组件类里`implements`生命周期钩子，以下是按顺序的钩子方法  
* ngOnChanges
>等于ng1的$scope.$watch函数   

发生在ngOnInit之前，或者绑定输入属性的值发生变化时
* ngOnInit
用于初始化，比如调用API获得数据
* ngDoCheck
用于变化检测，在每次变化监测发生时被调用，大多数情况下不会ngOnChange一起使用，ngDoCheck的粒度更小，能完成更灵活的变化监测
* ngAfterContentInit
在组件中使用`<ng-content>`将外部内容嵌入到组件视图后调用，只执行一次
* ngAfterContentChecked
在组件中使用`<ng-content>`将外部内容嵌入到组件视图后调用，或者每次变化监测时调用
* ngAfterViewInit
在Angular创建了组件的视图及其子组件视图后被调用  
>听起来像React的ComponentDidMount
* ngAfterViewChecked
在Angular创建了组件的视图及其子组件视图之后被调用一次，并且在每次子组件变化监测时也被调用
* ngOnDestroy
在销毁组件之前触发，那些订阅的观察者事件，绑定DOM事件，setTimeout、setInterval设置的计时器都应该在此被清除，以免引起内存泄漏

ng2的数据变化检测是通过NgZone服务掌控的
