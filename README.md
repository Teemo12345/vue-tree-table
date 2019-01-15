# vue-tree-table
## 功能
层级折叠，显示隐藏（非平铺数组，保持原有数组结构）
通过按钮向上、向下移动排序

## 使用方式
```html
<template>
  <div id="tree-table-demo">
    <h2>tree-table-demo</h2>
    <tree-table :head-data="headData" :body-data="bodyData" @on-sort="onSort" @on-operation="onOperation"></tree-table>
    <p v-if="swapMsg">{{swapMsg}}</p>
    <p v-if="operationMsg">{{operationMsg}}</p>
  </div>
</template>
<script>
import TreeTable from '@/components/TreeTable'
export default {
  name: 'tree-table-demo',
  components: {TreeTable},
  data () {
    return {
      headData: [
        {title: '#', key: 'index', type: 'text'},
        {title: 'title', key: 'title', type: 'text', extend: true},
        {title: 'sort', key: 'sort', type: 'sort-operation'},
        {title: 'operation', key: 'opt', type: 'operation', operations: [{type:'add', classType: 'primary', name: '编辑'}, {type:'del', classType: 'primary', name: '删除'}]}
      ],
      bodyData: [
        {title: '01-1级目录', children: [{title: '2级目录', children: [{title: '3级目录'}]}]},
        {title: '02-1级目录', children: [{title: '2级目录', children: [{title: '3级目录'}]}]},
        {title: '03-1级目录'}
      ],
      swapMsg: '',
      operationMsg: ''
    }
  },
  methods: {
    onSort(obj) {
      let {currRow, swapRow, direction} = obj
      this.swapMsg = `${currRow.title} / ${swapRow.title}->${direction} 位置交换成功`
    },
    onOperation (obj) {
      let {ele, row} = obj
      this.operationMsg = `${row.title}->${ele.type}-操作`
    }
  }
}
</script>
```
## API说明
### props
| 属性        | 说明           | 类型  |
| ------------- |:-------------:| -----:|
| bodyData| 显示的结构化数据|Array|
| headData| 表格列的配置描述|Array|

###methods
| 属性        | 说明           | 类型  |
| ------------- |:-------------:| -----:|
| @on-sort | 排序成功后方法传递|方法|
| @on-operation| 操作成功后方法传递|方法|

### headData 
| 属性        | 说明           | 类型 | 默认值|
| ----------- |:--------------:| -----:|-----:|
| title       | 列头显示文字       |String |#    |
| key         | 对应列内容的字段名 |String |#    |
| extend       | 是否为展开列             |Number |false    |
| type        |'text':普通字段|String|# |
| type        |'sort-operation':上下排序操作列|String|# |
| type        |'operation' 操作功能,  必填参数:operations:[{}]|String|#

## DEMO演示
### [DEMO](https://huanglong6828.github.io/vue-tree-grid/dist/index.html) 如果对您如果有帮助的话,给颗星谢谢


## 使用方式
``` bash
TreeTable 放入工程项目 例如 components/TreeTable,可直接深度更改。
```

## 本DEMO安装
```
yarn install
```

### Compiles and hot-reloads for development
```
yarn run serve
```

### Compiles and minifies for production
```
yarn run build
```

### Lints and fixes files
```
yarn run lint
```

