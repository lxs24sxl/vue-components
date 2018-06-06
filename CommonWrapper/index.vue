<template>
  <div class="common-wrapper">
    <div class="common-inner">
      <div class="top-wrapper">
        <h1>{{title}}</h1>
        <div class="btn-add-wrapper" v-if="hasClick">
          <el-button class="btn-add" type="success"  @click="extClick">{{clickTitle}}</el-button>
        </div>
      </div>
      <div class="content-wrapper">
        <slot></slot>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'commonwrapper',
  data() {
    return {
    }
  },
  props: {
    extClick: {
      type: Function,
      default: null
    },
    hasClick: {
      type:Boolean,
      default: false
    },
    title: {
      type: String,
      default: "默认标题"
    },
    clickTitle: {
      type: String,
      default: '默认点击标题'
    }
  }
}
</script>

<style lang="scss" scoped>
// 外部
.common-wrapper {
  width: 100%;
  // height: 500px;
  min-width: 50rem;
  .common-inner {
    padding-top: 10px;
    padding-left: 10px;
    padding-right: 10px;
    width: auto;
    height: 100%;
    .top-wrapper {
      height: 65px;
      width: 100%;
      position: relative;
      left: 0;
      top: 0;
      // background-color: #F2F2F2;
      line-height: 65px;
      h1 {
        margin: 0;
        padding: 0;
      }
      .btn-add-wrapper {
        position: absolute;
        right: 2rem;
        top: 0;
        .btn-add {
          width: 8rem;
        }
      }
    }
  }
}
</style>