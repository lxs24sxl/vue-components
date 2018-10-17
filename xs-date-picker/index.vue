<template>
  <div class="xs-date-picker">
    <el-input
      class="picker__input"
      placeholder="选择日期范围"
      :clearable="clearable"
      :readonly="readonly"
      :icon="iconClass"
      v-model="inputValue"
      @focus="handleFocus"
      @blur="handleBlur"
      @click="handleClick"
      :on-icon-click="handleClick"
    >
    </el-input>
    <!-- 弹出层 -->
    <div
      v-show="expandVisble"
      class="picker__content"
    >
      <p class="picker__content__title">日期范围:</p>
      <div class="el-data-picker__wrapper">
        <el-date-picker
          v-model="time.start"
          type="date"
          placeholder="开始日期"
          style="width: 120px"
        >
        </el-date-picker>
        <span>-</span>
        <el-date-picker
          v-model="time.end"
          type="date"
          placeholder="结束日期"
          style="width: 120px"
        >
        </el-date-picker>
      </div>
      <!-- 快捷日期 -->
      <shortcuts @pick="getDateRangeByShortcuts">
      </shortcuts>
      <div class="picker__content__footer">
        <el-button
          size="small"
          type="warning"
          @click="pubsubEvent('confirm')"
        >确定</el-button>
        <el-button
          plain
          size="small"
          type="primary"
          @click="pubsubEvent('cancel')"
        >取消</el-button>
      </div>
    </div>
    <!-- 遮盖层 -->
    <div
      v-show="expandVisble"
      class="picker__bg"
      @click="expandVisble = false"
    ></div>
  </div>
</template>

<script>
import shortcuts from './shortcuts'
import mixin from './mixin'
export default {
  mixins: [mixin],
  props: {
    clearable: {
      type: Boolean,
      default: false
    },
    readonly: {
      type: Boolean,
      default: true
    }
  },
  components: {
    shortcuts
  },
  computed: {
    iconClass() {
      if (this.clearable && this.inputValue) {
        return 'circle-cross'
      }
      return this.expandVisble ? 'caret-top' : 'caret-bottom'
    }
  },
  data() {
    return {
      inputValue: '',
      expandVisble: false,
      time: {
        start: '',
        end: ''
      }
    }
  },
  methods: {
    handleBlur(event) {
      this.$emit('blur', event)
    },
    handleClick(event) {
      if (this.clearable && this.inputValue) {
        this.time.start = ''
        this.time.end = ''
        this.inputValue = ''
        this.$emit('change', this.time)
      } else {
        this.expandVisble = true
      }
      this.$emit('click', event)
    },
    handleFocus(event) {
      this.expandVisble = true
      this.$emit('focus', event)
    },
    getDateRangeByShortcuts(date) {
      this.time.start = date[0]
      this.time.end = date[1]
    },
    pubsubEvent(command) {
      switch (command) {
        case 'confirm':
          let tempStart = this.time.start
          let tempEnd = this.time.end
          if (!tempStart) {
            this.$message({ message: '请选择开始日期~', type: 'info' })
            return
          }
          if (!tempEnd) {
            this.$message({ message: '请选择结束日期~', type: 'info' })
            return
          }
          if (tempEnd.getTime() < tempStart.getTime()) {
            this.$message({ message: '开始日期不能在结束日期之后~', type: 'info' })
            return
          }
          let start = this.formatDate(tempStart, 'yyyy-MM-dd')
          let end = this.formatDate(tempEnd, 'yyyy-MM-dd')
          this.inputValue = `${start} - ${end}`
          let time = {
            start,
            end
          }
          this.$emit('change', time)
          this.expandVisble = false
          break
        case 'cancel':
          this.expandVisble = false
          break
      }
    }
  }

}
</script>

<style lang="less">
@pickerContentLeft: 36%;
@pickerContentWidth: calc(~'100% + ' @pickerContentLeft);
.xs-date-picker {
  position: relative;
  .picker {
    &__input {
      transition: all .5s;
      input {
        cursor: pointer;
      }
      .el-input__icon {
        cursor: pointer;
      }
    }
    &__content {
      position: absolute;
      left: -@pickerContentLeft;
      z-index: 9999;
      margin-top: 8px;
      background-color: #fff;
      width: @pickerContentWidth;
      border: 1px solid #eee;
      box-shadow: 2px 2px 2px #eee;
      background-color: #fff;
      padding: 20px;
      z-index: 223;
      &__title {
        color: #999;
      }
      .el-data-picker__wrapper {
        display: flex;
        align-items: center;
        span {
          margin: 0 4px;
        }
      }
      ul,
      li {
        margin: 0;
        padding: 0;
      }
      &__shortcuts {
        display: flex;
        flex-wrap: wrap;
        [class*='shortcuts__item'] {
          min-width: calc(100% / 3);
          margin: 6px 0;
          padding-left: 5px;
          color: #666;
          cursor: pointer;
          transition: color 0.3s;
          &:hover {
            color: #17aabb;
          }
        }
        [class*='--large'] {
          min-width: 50%;
        }
      }
      &__footer {
        border-top: 1px solid #eee;
        margin-top: 4px;
        padding-top: 10px;
        .el-button.el-button--small {
          padding: 4px 10px;
        }
      }
    }
    &__bg {
      position: fixed;
      left: 0;
      top: 0;
      width: 100%;
      height: 100%;
      z-index: 222;
    }
  }
}
</style>
