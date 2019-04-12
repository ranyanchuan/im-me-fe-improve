# 贵冶项目前端开发中的一些技巧

> undefined 后跟着 . 回报 Uncaught TypeErro 错

##### 反例
```js
let listRow = {
    id: row.values.id.value,    //异常记录id
}
```

##### 正例
```js
const {values={}}=row; // 添加默认值 
const {id={}}=values;

let listRow = {
    id: id.value,    //异常记录id
}
```

> 大对象下 又是大对象  又是大对象 又是大对象 解构代码
##### 反例
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

##### 正例
公共方法抽取
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
            // 判断值对象是否有 value 属性
            if (data[key] && data[key].value) {
                value = data[key].value || "";
            }
            // 判断值对象是否有 dispaly 属性
            if (data[key] && data[key].display) {
                display = data[key].value || "";
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


> map 遍历返回的是一个数组，如果一个值的之后不会再改变 推荐使用 const 声明

##### 反例
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

##### 正例
```js
import {objDctValue} from "util";

const list=rows.map((row) => {
    const {values={}}=row;
    return objDctValue(values):
})
```
