<template>
  <div>
    <div class="d-flex justify-content-center pointer border b-radius-4" @click="showConfig = !showConfig"
         style="padding: 6px;margin-bottom: 8px">
      <div class="title">
        Vue Code Diff
        <el-button size="mini" type="text" @click="handleClearLocalStorage" style="margin-left: 10px">清除缓存</el-button>
      </div>
    </div>
    <div v-if="showConfig" class="border b-radius-4" style="padding: 6px">
      <el-form>
        <el-row :gutter="10" style="margin-bottom: 2px">
          <el-col :span="12">
            <el-form-item label="旧数据：">
              <el-input v-model="oldStr" type="textarea" :autosize="{minRows: 4, maxRows: 4}"
                        placeholder="请输入旧数据"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="12">
            <el-form-item label="新数据：">
              <el-input v-model="newStr" type="textarea" :autosize="{minRows: 4, maxRows: 4}"
                        placeholder="请输入新数据"></el-input>
            </el-form-item>
          </el-col>
        </el-row>
        <el-row :gutter="10" class="border b-radius-4" style="padding: 0 6px;margin: 0 2px">
          <el-col :span="8">
            <el-form-item label="展示效果：">
              <el-switch v-model="fotmat" active-text="line-by-line" inactive-text="side-by-side"></el-switch>
            </el-form-item>
          </el-col>
          <el-col :span="8">
            <el-form-item label="差异化范围：">
              <el-input-number size="mini" v-model="context"></el-input-number>
            </el-form-item>
          </el-col>
          <el-col :span="8">
            <el-form-item label="文件列表：">
              <el-switch size="mini" v-model="drawFileList" @click="drawFileList = !drawFileList"></el-switch>
            </el-form-item>
          </el-col>
          <el-col :span="8">
            <el-form-item label="不渲染：">
              <el-switch size="mini" v-model="renderNothingWhenEmpty"
                         @click="renderNothingWhenEmpty = !renderNothingWhenEmpty"></el-switch>
            </el-form-item>
          </el-col>
          <el-col :span="8">
            <el-form-item label="显示源代码：">
              <el-switch size="mini" v-model="isShowNoChange" @click="isShowNoChange = !isShowNoChange"></el-switch>
            </el-form-item>
          </el-col>
          <el-col :span="8">
            <el-form-item label="差异级别：">
              <el-button size="mini" v-model="diffStyle" @click="diffStyle = diffStyle === 'char' ? 'word' : 'char'">
                {{ diffStyle }}
              </el-button>
            </el-form-item>
          </el-col>
        </el-row>
      </el-form>
    </div>
    <code-diff
      :old-string="oldStr"
      :new-string="newStr"
      :context="context"
      :output-format="outputFormat"
      :drawFileList="drawFileList"
      :renderNothingWhenEmpty="renderNothingWhenEmpty"
      :diffStyle="diffStyle"
      :isShowNoChange="isShowNoChange"
    />
  </div>
</template>

<script>
import codeDiff from './lib/index.js'

export default {
  name: 'App',
  components: {
    codeDiff
  },
  data () {
    return {
      showConfig: true,
      oldStr: '',
      newStr: '',
      fotmat: false,
      context: 10000,
      diffStyle: 'word',
      isShowNoChange: true,
      drawFileList: false,
      renderNothingWhenEmpty: false
    }
  },
  computed: {
    outputFormat () {
      return this.fotmat ? 'line-by-line' : 'side-by-side'
    }
  },
  watch: {
    oldStr (v) {
      localStorage.setItem('oldStr', v)
    },
    newStr (v) {
      localStorage.setItem('newStr', v)
    }
  },
  created () {
    this.oldStr = localStorage.getItem('oldStr') || ''
    this.newStr = localStorage.getItem('newStr') || ''
  },
  methods: {
    handleClearLocalStorage (e) {
      e.stopPropagation()
      this.newStr = ''
      this.oldStr = ''
      localStorage.setItem('newStr', '')
      localStorage.setItem('oldStr', '')
    }
  }
}
</script>
<style>
.el-form-item {
  margin-bottom: 2px !important;
}

.title {
  padding: 4px 0 4px 0;
  font-size: 24px;
  font-weight: 700;
  color: cornflowerblue;
  font-family: "Comic Sans MS", cursive, sans-serif;
  text-align: center;
}
</style>
