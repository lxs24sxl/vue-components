<template>
  <div class="fixed-header-wrapper">
    <slot></slot>
  </div>
</template>

<script>
let timer = null
export default {
  props: {
    fixed: {
      type: Boolean,
      default: false
    },
    threshold: {
      type: [Number, String],
      default: 0
    }
  },
  watch: {
    fixed(v) {
      this.isFixed = v
    },
    isFixed(v) {
      this.$emit('update:fixed', v)
    }
  },
  data() {
    return {
      isFixed: false
    }
  },
  methods: {
    /**
     * @desc 获取滚动条距顶部的距离
     */
    getScrollTop() {
      return (
        (document.documentElement && document.documentElement.scrollTop) ||
        document.body.scrollTop
      )
    },
  },
  mounted() {
    // 脚本加载的期间提前检测
    // 条件预加载确保所有函数调消耗的时间相同。
    const addHandler = document.body.addEventListener
      ? function(target, eventType, handler) {
          target.addEventListener(eventType, handler, false)
        }
      : function(target, eventType, handler) {
          target.attachEvent('on' + eventType, handler)
        }
    let curScrollTop = 0                                    // 当前滚动高度
    const clientHeight = this.threshold || this.$el.children[0].clientHeight
    /**
     * 设置是否固定顶部菜单
     * @param {Number} curScrollTop 当前高度
     */
    const setFixed = (curScrollTop) => {
      this.isFixed = curScrollTop >= clientHeight
    }
    // 调用事件
    setFixed(this.getScrollTop())
    // 添加滚动事件
    addHandler(window, 'scroll', e => {
      if (timer) clearTimeout(timer)
      setFixed(this.getScrollTop())
      timer = setTimeout(() => {
        console.log('scrolling ends..')
      }, 150)
    })
  }
}
</script>

