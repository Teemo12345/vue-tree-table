<script>
const formatData = (data, parent) => {
  data.forEach((element, index) => {
    element.index = index + 1
    element.level = parent ? parent.level + 1 : 0
    element.isExtend = false
    element.isShow = parent === undefined
    if (Array.isArray(element.children) && element.children.length > 0) formatData(element.children, element)
  })
  return data
}
const childrenExtend = (extendData, state) => {
  extendData.forEach(item => {
    item.isShow = state
    if (item.children && !state) childrenExtend(item.children, state)
    if (item.children && state) childrenExtend(item.children, item.isExtend)
  })
}
export default {
  name: 'tree-table',
  props: {
    bodyData: {
      type: Array,
      default: () => []
    },
    headData: {
      type: Array,
      default: () => []
    }
  },
  data () {
    return {
      treeData: []
    }
  },
  methods: {
    onExtend (row) {
      if (!row.children) return
      row.isExtend = !row.isExtend
      childrenExtend(row.children, row.isExtend)
      this.$forceUpdate()
    },
    onSort (obj) {
      let {data, index, direction} = obj
      let swapIndex = direction === 'up' ? index - 1 : index + 1
      let currRow = Object.assign(data[index], {index: swapIndex + 1})
      let swapRow = Object.assign(data[swapIndex], {index: index + 1})
      this.$set(data, index, swapRow)
      this.$set(data, swapIndex, currRow)
      this.$emit('on-sort', {currRow, swapRow, direction})
    },
    onOperation (obj) {
      this.$emit('on-operation', obj)
    }
  },
  watch: {
    'bodyData.length': function () {
      this.treeData = formatData(this.bodyData)
    }
  },
  mounted () {
    if (this.bodyData.length > 0) this.treeData = formatData(this.bodyData)
  },
  render () {
    let columnEach = (row, data, index) => {
      return this.headData.map(column => {
        return [
          <td >
            <div v-show={column.type === 'text'}>
              <span class="" v-show={column.extend} style={{'padding-left': (row.level * 10) + 'px'}} on-click={e => this.onExtend(row, e)}>
                <span v-show={row.children}>{row.isExtend ? '📂' : '📁'}</span>
                <span v-show={!row.children}>📓</span>
                {row[column.key]}
              </span>
              <span v-show={!column.extend}>{row[column.key]}</span>
            </div>
            <div v-show={column.type === 'sort-operation'}>
              <button disabled={index === 0} on-click={e => this.onSort({data, index, direction: 'up'}, e)}>⬆︎</button>
              <button disabled={index === data.length - 1} on-click={e => this.onSort({data, index, direction: 'down'}, e)}>⬇︎</button>
            </div>
            <div v-show={column.type === 'operation'}>
              {
                column.operations && column.operations.map(ele => {
                  return [
                    <button class={ele.classType} on-click={e => this.onOperation({ele, row}, e)}>{ele.name}</button>
                  ]
                })
              }
            </div>
          </td>
        ]
      })
    }
    let rowEach = (data) => {
      return data.map((row, index) => {
        return [
          <tr v-show={row.isShow}>
            {columnEach(row, data, index)}
          </tr>,
          Array.isArray(row.children) && rowEach(row.children)
        ]
      })
    }
    return (
      <div id="tree-table">
        <table cellspacing="0" cellpadding="0" border="0">
          <thead>
            <tr>
            {
              this.headData.length > 0 ? this.headData.map(item => {
                return [
                  <th>{item.title}</th>
                ]
              }) : <th>请先绑定头部数据！</th>
            }
            </tr>
          </thead>
          <tbody>
            {
              rowEach(this.treeData)
            }
          </tbody>
        </table>
      </div>
    )
  }
}
</script>
