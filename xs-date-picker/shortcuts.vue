<template>
  <div>
    <p v-if="shortcuts.length" class="picker__content__title">快捷日期:</p>
      <ul v-if="shortcuts.length" class="picker__content__shortcuts">
        <li 
          v-for="(shortcut, key) in shortcuts"
          :key="key"
          :class="shortcut.size!=='large'?'shortcuts__item':'shortcuts__item--large'" 
          @click="handleShortcutClick(shortcut)">
          {{shortcut.text}}
        </li>
      </ul>
  </div>
</template>

<script>
export default {
  data() {
    const day = 3600 * 1000 * 24
    /**
     * 根据 距离当前天数长度 来获取范围
     * @param {Number|String} AddDayCount 天数
     * @return {Array} 数组第一项为起始时间， 第二项为结束时间
     */
    const getDateRange = (AddDayCount) => {
      const end = new Date()
      const start = new Date()
      start.setTime(start.getTime() + day * +AddDayCount)
      return [start, end]
    }
    /**
     * 利用月数获得次月的天数
     * @param {Number} year 年
     * @param {Number} month 月
     * @return {Number} 天数
     */
    const getDaysByMonth = (year, month) => {
      return new Date(+year, +month + 1, 0).getDate()
    }
    /**
     * 根据命令来获取范围
     * @param {String} command 命令
     * @return {Array} 数组第一项为起始时间， 第二项为结束时间
     */
    const getDateRangeByCommand = (command) => {
      const start = new Date()
      const end = new Date()
      /**
       * 特殊处理的命令函数
       * @param {Date} start 开始时间
       * @param {Date} end 结束时间
       * @param {Number} endDay 日期
       */
      const handleSpecialCommand = (start, end, endDay) => {
        // 例如： 获得距离上一个周六(6)的天数
        let endAddDayCount = endDay - end.getDay() - 7
        end.setTime(getDateRange(endAddDayCount)[0])
        start.setTime(getDateRange(endAddDayCount - 6)[0])
      }
      switch (command) {
        case '上月':
          let AddDayCount = getDaysByMonth(start.getFullYear(), start.getMonth() - 1)
          start.setTime(start.getTime() + day * +(-AddDayCount))
          break
        case '上周(周日至周六)':
          handleSpecialCommand(start, end, 6)
          break
        case '上周(周一至周日)':
          handleSpecialCommand(start, end, 7)
          break
      }
      return [start, end]
    }
    return {
      shortcuts: [
        {
          text: '昨天',
          onClick(picker) {
            picker.$emit('pick', getDateRange(-1))
          }
        }, {
          text: '前天',
          onClick(picker) {
            picker.$emit('pick', getDateRange(-2))
          }
        }, {
          text: '过去7天',
          onClick(picker) {
            picker.$emit('pick', getDateRange(-7))
          }
        }, {
          text: '过去14天',
          onClick(picker) {
            picker.$emit('pick', getDateRange(-14))
          }
        }, {
          text: '过去30天',
          onClick(picker) {
            picker.$emit('pick', getDateRange(-30))
          }
        }, {
          text: '上月',
          onClick(picker) {
            picker.$emit('pick', getDateRangeByCommand('上月'))
          }
        }, {
          text: '过去90天',
          onClick(picker) {
            picker.$emit('pick', getDateRange(-90))
          }
        }, {
          text: '过去半年',
          onClick(picker) {
            picker.$emit('pick', getDateRange(-180))
          }
        },
        {
          text: '上周(周日至周六)',
          size: 'large',
          onClick(picker) {
            picker.$emit('pick', getDateRangeByCommand('上周(周日至周六)'))
          }
        },
        {
          text: '上周(周一至周日)',
          size: 'large',
          onClick(picker) {
            picker.$emit('pick', getDateRangeByCommand('上周(周一至周日)'))
          }
        }
      ]
    }
  },
  methods: {
    /**
     * 监听快捷日期的点击
     * @param {Object} shortcut 快捷日期
     */
    handleShortcutClick(shortcut) {
      if (shortcut.onClick) {
        shortcut.onClick(this)
      }
    }
  }
}
</script>
