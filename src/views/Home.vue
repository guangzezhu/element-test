<template>
  <div class="home">
    <div class="left">
      <el-upload
        class="upload-demo"
        action=""
        :file-list="fileList"
        :auto-upload="false"
        :on-change="handleChange">
        <el-button size="small" type="primary">点击上传</el-button>
      </el-upload>
    </div>
    <div class="right">
      <div ref="container" class="container">
        <!-- <el-button class="icon-one" @click="onClickOne">Icon</el-button>
        <i class="el-icon-full-screen full-screen-icon" @click="onFullScreen"/>
        <dialog-one ref="DialogOne" /> -->
      </div>
    </div>
  </div>
</template>

<script>
import DialogOne from './dialog-one.vue'
import SparkMD5 from 'spark-md5'

export default {
  name: 'Home',
  components: {
    // eslint-disable-next-line vue/no-unused-components
    DialogOne
  },
  data () {
    return {
      isFullScreen: false,
      fileList: [],
      // md5
      // 1048576
      chunkSize: 52428800,
      count: 0
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
    },
    incrementalMD5 (file) {
      return new Promise((resolve, reject) => {
        const fileReader = new FileReader()
        const spark = new SparkMD5.ArrayBuffer()
        const chunks = Math.ceil(file.size / this.chunkSize)
        let currentChunk = 0

        fileReader.onload = event => {
          spark.append(event.target.result) // Append array buffer
          ++currentChunk
          currentChunk < chunks ? this.loadNext(fileReader, file, currentChunk, this.chunkSize) : resolve(spark.end()) // Compute hash
        }

        fileReader.onerror = () => reject(fileReader.error)

        this.loadNext(fileReader, file, currentChunk, this.chunkSize)
      })
    },
    loadNext (fileReader, file, currentChunk, chunkSize) {
      const start = currentChunk * chunkSize
      const end = start + chunkSize >= file.size ? file.size : start + chunkSize
      fileReader.readAsArrayBuffer(File.prototype.slice.call(file, start, end))
      // console.log(`---> calculate md5, chunk count: ${++this.count}, chunk size: ${this.chunkSize} byte (20M)`)
    },
    async handleChange (file) {
      const startTime = new Date().valueOf()
      this.incrementalMD5(file.raw)
        .then(md5 => {
          const endTime = new Date().valueOf()
          console.log('=== md5: ', md5)
          console.log('=== time: ' + (endTime - startTime) + 'ms')
        })
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

.upload-demo {
  margin: 40px 0 0 40px;
}
</style>
