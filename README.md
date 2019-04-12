# 贵冶项目前端开发中的一些技巧

> undefined 后跟着 . 回报 Uncaught TypeErro 错

# 反例
```js
let listRow = {
    id: row.values.id.value,    //异常记录id
    expname: row.values.expname.value,  //异常名称
}
```

# 正例
```js
const {values={}}=row; // 添加默认值 
const {id={},expname={},expsource={}}=values;

let listRow = {
    id: id.value,    //异常记录id
    expname: expname.value,  //异常名称
    expsource: expsource.value,  //异常来源
}
```

> map 遍历返回的是一个数组，

# 反例
```js
rows.map((row) => {
    let listRow = {
        id: row.values.id.value,    //异常记录id
        expname: row.values.expname.value,  //异常名称
        expsource: row.values.expsource.value,  //异常来源
        exptype: row.values.exptype.value,  //异常类型
        expstatus: row.values.expstatus.value,  //异常状态
    }
    list.push(listRow);
})
```

# 正例
```js
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
    }
    list.push(listRow);
})
```
