<template>
  <div class="hello">
    <button @click="isShow = !isShow">显示/隐藏</button>
    <transition-group name="hello" appear>
      <h1 v-show="isShow" key='1'>这是一个动画效果</h1>
      <h1 v-show="isShow" key='2'>Hello</h1>
    </transition-group> 
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data() {
    return {
      msg: '动画',
      isShow: true
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">

/* 进入的起点 离开的终点  */
.hello-enter,
.hello-leave-to {
  transform: translateX(-100%);
}
/* 进入的整个过程 离开的整个过程 */
.hello-enter-active,
.hello-leave-active {
  transition: 0.5s linear;
}
/* 进入的终点 离开的起点 */
.hello-enter-to,
.hello-leave {
  transform: translateX(0);
}

h1 {
  width: 500px;
  height: 50px;
  line-height: 50px;
  background-color: aquamarine;
}
</style>
