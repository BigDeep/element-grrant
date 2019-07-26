<template>
  <div class="grantt">
    <el-table :data="tableData"
              border
              :cell-class-name="cellClassName"
              @row-click="vvvv"
              @cell-mouse-enter="enter"
              @cell-mouse-leave="leave">
      <el-table-column v-for="(item,index) in columns"
                       :key="index"
                       :prop="item.prop"
                       :label=item.label
                       fixed
                       :width="item.width">
      </el-table-column>
      <el-table-column v-for="(item) in getRangeDate"
                       :key="item"
                       :prop="item"
                       :label="item"
                       :width="85">

      </el-table-column>
    </el-table>
  </div>
</template>
<script>
export default {
  name: 'gantt',
  components: {},
  props: {
    ganttWidth: {
      type: String,
      default: ''
    },
    dateType: {
      type: Number,
      default: 1
    },
    dateRange: {
      type: Array,
      default: () => ['2019-06-11', '2019-06-18']
    },
    tableDatax: {
      type: Array,
      default: () => []
    },
    columns: {
      type: Array,
      default: () => [
        {
          prop: 'id',
          label: '编号',
          width: '60px'
        },
        {
          prop: 'taskName',
          label: '任务名称',
          width: '200px'
        },
        {
          prop: 'startTime',
          label: '开始时间'
        },
        {
          prop: 'useTime',
          label: '天数'
        },
        {
          prop: 'follower',
          label: '负责人'
        }
      ]
    }
  },
  data() {
    var getRangeDate = (stime, etime) => {
      // 初始化日期列表，数组
      var diffdate = []
      var i = 0
      // 开始日期小于等于结束日期,并循环
      while (stime <= etime) {
        diffdate[i] = stime

        // 获取开始日期时间戳
        var stime_ts = new Date(stime).getTime()
        // 增加一天时间戳后的日期
        var next_date = stime_ts + 24 * 60 * 60 * 1000

        // 拼接年月日，这里的月份会返回（0-11），所以要+1
        var next_dates_y = new Date(next_date).getFullYear() + '-'
        var next_dates_m =
          new Date(next_date).getMonth() + 1 < 10
            ? '0' + (new Date(next_date).getMonth() + 1) + '-'
            : new Date(next_date).getMonth() + 1 + '-'
        var next_dates_d =
          new Date(next_date).getDate() < 10
            ? '0' + new Date(next_date).getDate()
            : new Date(next_date).getDate()

        stime = next_dates_y + next_dates_m + next_dates_d

        // 增加数组key
        i++
      }
      return diffdate
    }
    return {
      format: value => {
        const h = this.$createElement
        return h('div', {
          on: {
            click: this.vvvv
          }
        })
      },
      getRangeDate: getRangeDate(this.dateRange[0], this.dateRange[1]),
      tableData: [
        {
          id: 1,
          taskName: '任务名称',
          startTime: '开始时间',
          useTime: '2',
          follower: '李xx'
        },
        {
          id: 2,
          taskName: '任务名称',
          startTime: '开始时间',
          useTime: '3',
          follower: '庄x'
        },
        {
          id: 3,
          taskName: '任务名称',
          startTime: '开始时间',
          useTime: '4',
          follower: '项xx'
        },
        {
          id: 4,
          taskName: '任务名称',
          startTime: '开始时间',
          useTime: '5',
          follower: '李xx'
        },
        {
          id: 5,
          taskName: '任务名称',
          startTime: '开始时间',
          useTime: '5',
          follower: '李xx'
        },
        {
          id: 6,
          taskName: '任务名称',
          startTime: '开始时间',
          useTime: '5',
          follower: '李xx'
        },
        {
          id: 7,
          taskName: '任务名称',
          startTime: '开始时间',
          useTime: '5',
          follower: '李xx'
        },
        {
          id: 8,
          taskName: '任务名称',
          startTime: '开始时间',
          useTime: '5',
          follower: '李xx'
        }
      ],
      current: false
    }
  },
  mounted() {
    // const granttTd = document.getElementsByClassName('grantt')[0]
    // let currentRow = ''
    // granttTd.addEventListener('mousedown', event => {
    //   if (event.target.className.indexOf('grantt-td') !== -1) {
    //     currentRow = event.target.parentElement
    //     console.log(event.target.parentElement)
    //   }
    // })
    // granttTd.addEventListener('mouseup', event => {
    //   if (
    //     event.target.className.indexOf('grantt-td') !== -1 &&
    //     event.target.parentElement === currentRow
    //   ) {
    //     console.log(event.target)
    //     console.log('同一行move')
    //   }
    // })
  },
  methods: {
    enter(row, column, cell, event) {
      if (event.which === 0) {
        this.current = !row[column.label]
      }
      if (event.which === 1) {
        this.$set(row, column.label, this.current)
      }
    },
    leave(row, column, cell, event) {
      if (event.which === 1) {
        this.$set(row, column.label, this.current)
      }
    },
    vv() {
      console.log(50)
    },
    vvvv(row, column, event) {
      this.$set(row, column.label, !row[column.label])
    },
    cellClassName(params) {
      const { row, column } = params
      if (row[column.label] === true) {
        return 'grantt-td grantt-selected'
      } else {
        return 'grantt-td'
      }
    }
  }
}
</script>
<style lang="scss">
.grantt {
  .grantt-selected {
    background-color: #00b8ff !important;
  }
}
.el-table__body tr.hover-row > td {
  background-color: none !important;
}
</style>

