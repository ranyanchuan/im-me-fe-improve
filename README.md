# 贵冶项目前端开发中的一些技巧

##### 1、undefined 后跟着 `.` 会报 Uncaught TypeErro 错

`反例`
```js
let listRow = {
    id: row.values.id.value,    //异常记录id
}
```

`正例`
```js
const {values={}}=row; // 添加默认值 
const {id={}}=values;

let listRow = {
    id: id.value,    //异常记录id
}
```
***
#####  2、 解构代码 `row` 对象下的 `value`
`反例`
```js
let listRow = {
    id: row.values.id.value,    //异常记录id
    expname: row.values.expname.value,  //异常名称
    expsource: row.values.expsource.value,  //异常来源
    exptype: row.values.exptype.value,  //异常类型
    expstatus: row.values.expstatus.value,  //异常状态
    ...
}

```
`正例 公共方法抽取`
```js
/**
 * 解构 对象下是对象
 * 将一个对象A，A对象的属性值又是一个对象B，将A 对象key 和 A对象的属性值B对象的属性value和display 值 进行封装一个新对象
 * @param data 解构对象, childrenKey 子对象key 
 *
 */
export function objDctValue(data,childrenKey) {
   const result = {};
        // 如果 childrenKey 非空
        if(childrenKey){
            for (const key in data) {
                let value = "";
                // 判断值对象是否有 value 属性
                if (data[key] && data[key][childrenKey]) {
                    value = data[key][childrenKey] || "";
                }
                result[key] = value; // 值
            }
            return result;
        }
        // childrenKey 值为空 取 value 和 display
        for (const key in data) {
            let value = "";
            let display="";
            // 判断值对象是否有 value 或者 display 属性
            if (data[key] && (data[key].value || data[key].display)) {
                value = data[key].value || '';
                display = data[key].dispaly || '';
            }
            result[key] = value; // 值
            result[key+'_name'] = display; // 名称
        }
        return result;
}
```
```js
import {objDctValue} from "util";

const {values={}}=row;
let listRow =objDctValue(values,'value');

```
***
##### 3、`rows` 数组解构， `map` 遍历本身返回的是一个数组，如果一个变量的值之后不会再改变 推荐使用 `const` 声明

`反例`
```js
const list=[];
rows.map((row) => {
    let listRow = {
        id: row.values.id.value,    //异常记录id
        expname: row.values.expname.value,  //异常名称
        expsource: row.values.expsource.value,  //异常来源
        exptype: row.values.exptype.value,  //异常类型
        expstatus: row.values.expstatus.value,  //异常状态
        happentime: row.values.happentime.value,    //发现时间
        discover_dept: row.values.discover_dept.value,  //发现单位
        discover_dept_name: row.values.discover_dept.display,  //发现单位名称
        discover_person: row.values.discover_person.value,  //发现人
        discover_person_name: row.values.discover_person.display,  //发现人名称
        discover_post: row.values.discover_post.value,  //发现岗位
        discover_post_name: row.values.discover_post.display,  //发现岗位名称
        deal_post: row.values.deal_post.value,  //处理岗
        deal_post_name: row.values.deal_post.display,  //处理岗名称
        expcontent: row.values.expcontent.value,    //异常描述
        picpaths: row.values.picpaths.value,    //图片
        ...
    }
    list.push(listRow);
})
```

`正例`
```js
import {objDctValue} from "util";

const list=rows.map((row) => {
    const {values={}}=row;
    return objDctValue(values):
})
```

#### 4、tootip 传参问题 数组迭代问题
`反例`
```js
<div className='dross un-slagnormal'>
{
    aisle.map((a) => {
        return (
            <p key={a.index} className='dross-item'>
                <span className='slag-num'>{a.index + 1}</span>
                <Tooltip
                    overlay={
                        this.tootip({
                            pk_slagnum_name: a.pk_slagnum_name,
                            intime: a.intime,
                            watertiom: a.watertiom,
                            status: a.status,
                        })
                    }
                    placement="right"
                >
                    <span className='slag-pos' style={{ background: colorArr[a.status] }}>{a.pk_slagnum_name}</span>
                </Tooltip>

            </p>
        )
    })
}
</div>
```

`正例`
```js
<div className='dross un-slagnormal'>
{
    // 判断是否是数组
    Array.isArray(aisles) &&  aisles.map((aisle) => {
    const {status,pk_slagnum_name,index}=aisle || {};
    const style={ background: colorArr[status] };
    return (
        <p key={index} className='dross-item'>
            <span className='slag-num'>{index + 1}</span>
            <Tooltip overlay={this.tootip(aisle)} placement="right" >
                <span className='slag-pos' style={style}>{pk_slagnum_name}</span>
            </Tooltip>
        </p>
    )
 })
}
</div>
```
#### 5、import 引入地方库
`反例`

```js
import zhCN from "../../../../../node_modules/rc-calendar/lib/locale/zh_CN";
import '../../../../../node_modules/bee-datepicker/build/DatePicker.css';
```

`正例`
```js
import zhCN from "rc-calendar/lib/locale/zh_CN";
import 'bee-datepicker/build/DatePicker.css';
```



