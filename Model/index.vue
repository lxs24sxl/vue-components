<template>
  <div class="model-wrapper" v-if="showModel" >
    <div class="center-wrapper">
      <div class="title">{{title}}</div>
      <slot></slot>
      <div class="btn-wrapper">
        <el-button 
          type="default" 
          class="btn" 
          @click.native.prevent="cancel"
          >取消</el-button>
        <el-button 
          type="primary" 
          class="btn"
          :loading="loading" 
          @click.native.prevent="confirm"
          >确定</el-button>
      </div>
    </div>
    <div class="model-bg" @click="cancel"></div>
  </div>
  
</template>

<script>
export default {
  data() {
    return {

    }
  },
  props: {
    showModel: {
      type: Boolean,
      default: false
    },
    title: {
      type: String,
      default: '默认标题'
    },
    cancel: {
      type: Function,
      default: null
    },
    confirm: {
      type: Function,
      default: null
    },
    loading: {
      type: Boolean,
      default: false
    }
  }
}
</script>

<style lang="scss" scoped>
.model-wrapper {
  position: fixed;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  z-index: 9999;
  .model-bg {
    width: 100%;
    height: 100%;
    background-color: #000;
    opacity: 0.4;
  }
}
.center-wrapper {
  position: fixed;
  left: 50%;
  top: 50%;
  transform: translate(-50%,-50%);
  width: 500px;
  // height: 500px;
  border-radius: 10px;
  background-color: white;
  // border: 1px solid #eee;
  z-index: 100;
  .title {
    width: 100%;
    padding: 0 20px;
    text-align: center;
    height: 60px;
    line-height: 60px;
    font-size: 18px;
    border-top-left-radius: 10px;
    border-top-right-radius: 10px;
    color: #fff;
    background-color: #000;
  }
  .el-form-wrapper {
    padding: 20px;
  }
  .btn-wrapper {
    width: 100%;
    text-align: center;
    font-size: 0;
    .btn {
      border-radius: 0;
      margin: 0;
      height: 50px;
      display: inline-block;
      width: 50%;
      &:first-child{
        border-bottom-left-radius: 10px;
      }
      &:last-child {
        border-bottom-right-radius: 10px;
      }
    }
  }
}
</style>