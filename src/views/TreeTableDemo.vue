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
<style>
#tree-table-demo table{width:100%;}
#tree-table-demo table th,
#tree-table-demo table td{border-bottom:1px solid #ccc;padding:5px 10px;text-align:left;border-right:1px solid #ccc;}
#tree-table-demo table td:last-child{border-right:0;}
#tree-table-demo table th:last-child{border-right:0;}
#tree-table-demo table th{background-color: #f1f1f1;}
p{color:cadetblue;}
</style>
