<template>
  <div class="component">
    <div class="v-segment">
      <div
        @click="handleClick(index)"
        :style="selectedStyle(index)"
        :class="{selected: selectedIndex === index}"
        class="v-segment-item"
        v-for="(item,index) in info">
        {{item}}
      </div>
    </div>
  </div>
</template>

<script>
  import {VueExtend} from 'godspen-lib'

  export default {
    mixins: [VueExtend.mixin],
    name: 'v-segment',
    label: process.env.LABEL,
    style: process.env.STYLE,
    stack: false, // 是否是堆叠模式 ，true：孩子元素会按楼层一个个向下排布， false: 孩子元素绝对定位，随意放置位置
    childLimit: 9999,  // 孩子元素最大限制数
    leaf: false, // 是否是叶子节点，为true的时候该节点下面不能添加节点
    props: {
      datasourcekey: {
        type: String,
        default: '',
        editor: {
          ignore: true
        }
      },
      data: {
        type: Array,
        default () {
          return ['Segment1', 'Segment2', 'Segment3']
        },
        editor: {
          ignore: true
        }
      },
      config: {
        type: Object,
        default () {
          return {
            tintColor: '#2DB7F5',
            fn: []
          }
        },
        editor: {
          ignore: true
        }
      }
    },
    computed: {
      info () {
        var data = this.data
        if (this.datasourcekey) {
          var dataStr = this.dataHubGet && this.dataHubGet(this.datasourcekey)
          try {
            data = JSON.parse(dataStr)
          } catch (e) {
            console.log('error v-accordion', '获取数据解析json对象异常')
            data = []
          }
        }
        return data
      }
    },
    data () {
      return {
        selectedIndex: -1
      }
    },
    mounted () {
      this.handleClick(0)
    },
    editorMethods: {
    },
    methods: {
      selectedStyle ($index) {
        if (this.selectedIndex === $index) {
          return {
            color: '#fff',
            borderColor: this.config.tintColor,
            backgroundColor: this.config.tintColor
          }
        } else {
          return {
            color: this.config.tintColor,
            borderColor: this.config.tintColor,
            backgroundColor: 'transparent'
          }
        }
      },
      handleClick ($index) {
        if (this.selectedIndex === $index) {
          return
        }
        this.selectedIndex = $index
        console.log(`selectedIndex:${$index}`)
        if (this.config.fn[$index]) this.oncallExecute(this.config.fn[$index], [this.info[$index] || '', $index])
      }
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus" type="text/stylus" scoped>
  .component {
    width: 100%;
    height: 100%;
    .v-segment {
      display: flex;
      border-radius: 5px;
      overflow: hidden;
      min-height: 27px;
      opacity: 1;
      .v-segment-item {
        display: flex;
        width: 100%;
        flex: 1;
        justify-content: center;
        align-items: center;
        color: #2DB7F5;
        font-size: 14px;
        line-height: 1;
        transition: background .2s;
        position: relative;
        border: 1px solid #2DB7F5;
        box-sizing: border-box;
        border-left-width: 0;

        &:first-child {
          border-left-width: 1px;
          border-radius: 5px 0 0 5px;
        }
        &:last-child {
          border-radius: 0 5px 5px 0;
        }

        &.selected {
          color: #fff;
        }
      }
    }
  }
</style>
