<template>
    <el-dialog
        title="提示"
        :visible.sync="dialogVisible"
        width="30%">
        <el-form :model="form">
            <el-form-item label="item">
                <el-select v-model="form.select" :popper-append-to-body="false" placeholder="请选择">
                    <el-option
                    v-for="item in options"
                    :key="item.value"
                    :label="item.label"
                    :value="item.value">
                    </el-option>
                </el-select>
            </el-form-item>
            <el-form-item label="item">
                <el-date-picker
                    v-model="form.date"
                    :append-to-body="false"
                    type="daterange"
                    range-separator="至"
                    start-placeholder="开始日期"
                    end-placeholder="结束日期">
                </el-date-picker>
            </el-form-item>
        </el-form>
        <span slot="footer" class="dialog-footer">
            <el-button @click="dialogVisible = false">取 消</el-button>
            <el-button type="primary" @click="dialogVisible = false">确 定</el-button>
        </span>
    </el-dialog>
</template>

<script>
export default {
  name: 'DialogOne',
  props: {
    fullscreen: {
      type: Boolean,
      default: false
    }
  },
  data () {
    return {
      dialogVisible: false,
      form: {
        select: null,
        date: null
      },
      options: [{
        value: '选项1',
        label: '黄金糕'
      }, {
        value: '选项2',
        label: '双皮奶'
      }, {
        value: '选项3',
        label: '蚵仔煎'
      }, {
        value: '选项4',
        label: '龙须面'
      }, {
        value: '选项5',
        label: '北京烤鸭'
      }]
    }
  },
  methods: {
    show () {
      this.dialogVisible = true
    }
  }
}
</script>

<style lang="scss" scoped>

</style>
