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
