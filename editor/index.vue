<template>
  <div class="component-editor">
    <el-card class="box-card">
      <div slot="header" class="clearfix">
        <span>分段器数据</span>
      </div>
      <el-form ref="form" label-width="60px" size="mini">
        <el-form-item label="使用数据源:" label-width="100px">
          <el-checkbox border size="mini" v-model="usedatasource"></el-checkbox>
        </el-form-item>
      </el-form>
      <el-form v-if="usedatasource" ref="form" label-width="60px" size="mini">
        <el-form-item label="数据源:" label-width="100px">
          <el-input placeholder="填写数据总线的key" v-model="componentInfo.datasourcekey"></el-input>
        </el-form-item>
      </el-form>
      <el-table v-else :data="componentInfo.data" style="width: 100%">
        <el-table-column align="center" placeholder="请输入分段内容" label="分段内容">
          <template slot-scope="scope">
            <el-input v-model="componentInfo.data[scope.$index]"></el-input>
          </template>
        </el-table-column>
        <el-table-column align="center" label="操作">
          <template slot-scope="scope">
            <el-button size="mini" type="text" @click="handleAdd(scope.$index)">+</el-button>
            <el-button size="mini" type="text" @click="handleDelete(scope.$index)">-</el-button>
          </template>
        </el-table-column>
      </el-table>
    </el-card>
    <el-card style="margin-top: 20px;">
      <div slot="header" class="clearfix">
        <span>基础配置</span>
      </div>
      <el-form ref="form" size="mini">
        <el-form-item label="分段器主色调:" label-width="100px">
          <el-color-picker v-model="componentInfo.config.tintColor" show-alpha></el-color-picker>
        </el-form-item>
        <!--<el-form-item label="点击操作" prop="fn">-->
          <!--<attr-function :content.sync="componentInfo.config.fn"></attr-function>-->
        <!--</el-form-item>-->
      </el-form>
    </el-card>
  </div>
</template>

<script>
  export default {
    name: 'maliangeditor',
    props: {
      componentInfo: { // 固定字段，收集所有属性值
        type: [Object],
        default () {
          return {
            datasourcekey: '',
            data: ['Segment1', 'Segment2', 'Segment3'],
            config: {
              tintColor: '#2DB7F5',
              fn: []
            }
          }
        }
      }
    },
    data () {
      return {
        usedatasource: false
      }
    },
    watch: {
      'usedatasource': {
        handler (v) {
          if (!v) {
            this.componentInfo.datasourcekey = ''
          }
        },
        immediate: true
      }
    },
    mounted: function () {
    },
    methods: {
      handleAdd (i) {
        this.componentInfo.data.splice(i + 1, 0, '')
      },
      handleDelete (i) {
        this.componentInfo.data.splice(i, 1)
      }
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus" type="text/stylus" scoped>
</style>