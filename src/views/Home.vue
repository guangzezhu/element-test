<template>
  <div class="home">
    <div class="left">
      left
    </div>
    <div class="right">
      <div ref="container" class="container">
        <el-button class="icon-one" @click="onClickOne">Icon</el-button>
        <i class="el-icon-full-screen full-screen-icon" @click="onFullScreen"/>
        <dialog-one ref="DialogOne" />
      </div>
    </div>
  </div>
</template>

<script>
import DialogOne from './dialog-one.vue'

export default {
  name: 'Home',
  components: {
    DialogOne
  },
  data () {
    return {
      isFullScreen: false
    }
  },
  methods: {
    onFullScreen () {
      this.isFullScreen = !this.isFullScreen

      if (this.isFullScreen) {
        this.$refs.container.requestFullscreen()
      } else {
        document.exitFullscreen()
      }
    },
    onClickOne () {
      this.$refs.DialogOne.show()
    }
  }
}
</script>

<style lang="scss" scoped>
.home {
  display: flex;
  height: 100vh;
}

.left {
  flex: 1;
  background: #ddd;
}

.right {
  flex: 3;
}

.container {
  position: relative;
  background: #777;
  height: 50%;
  width: 50%;
}

.icon-one {
  position: absolute;
  right: 20px;
  top: 20px;
}

.full-screen-icon {
  cursor: pointer;
  position: absolute;
  right: 20px;
  bottom: 20px;
}
</style>
