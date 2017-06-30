<template>
  <div :id="domId"></div>
</template>

<script type="text/ecmascript-6">

  export default {
    mounted () {
      this.domId = this.getUID(this.domId);
      let config = {};

      // 获取配置项
      Object.assign(config, this.defaultConfig, this.config);
      config.toolbars = this.toolbars.length > 0 ? this.toolbars : this.defaultToolbars;

      // 异步初始化，避免 DOM 的 Id 还未渲染，而导致初始化找不到 DOM 报错
      this.$nextTick(() => {
        this.init(this.domId, config);
      });
    },
    props: {
      content: {
        type: String,
        default: ''
      },
      config: {
        type: Object,
        default: () => {
            return {};
        }
      },
      toolbars: {
        type: Array,
        default: () => {
          return [];
        }
      }
    },
    data () {
      return {
        ue: null,
        domId: 'J_UEditor',
        defaultToolbars: [ // 默认工具栏
          [
            'bold', // 加粗
            'italic', // 斜体
            'fontsize', // 字号
            'underline', // 下划线
            'forecolor', // 字体颜色
            'justifyleft', // 居左对齐
            'justifycenter', // 居中对齐
            'justifyright', // 居右对齐
            'justifyjustify', // 两端对齐
            'lineheight', // 行间距
            'removeformat', // 清除格式
            'source' // 查看源码
          ]
        ],
        defaultConfig: { // ueditor 配置项
          serverUrl: '', // 上传地址
          zIndex: 1050, // 默认为 900，但是 modal-backdrop 的 z-index = 1040，故不可比它小
          initialFrameWidth: '100%', // 初始化宽度
          initialFrameHeight: 200, // 初始化高度 px
          elementPathEnabled: false, // 是否显示元素路径
          autoClearinitialContent: false, // 是否自动清除编辑器初始内容
          wordCountMsg: '当前已输入{#count}个字符, 您还可以输入{#leave}个字符&emsp;' // 字数统计
        }
      }
    },
    methods: {
      getUID (prefix) {
        do {
            prefix += ~~(Math.random() * 1000000);
        }
        while (document.getElementById(prefix));

        return prefix;
      },
      init (domId, config) {
        this.ue = UE.getEditor(domId, config);
        this.bind();
      },
      bind () {
        // 当编辑器初始化完成，设置 content
        this.ue.ready(() => {
            this.ue.setContent(this.content);
        });

        // 当编辑内容发生改变，emit 一个 change 事件，并传进当前的内容
        this.ue.addListener('contentChange', () => {
          this.$emit('change', this.getContent());
        });
      },
      reset () {
        this.ue.reset();
        this.ue.setContent('');
      },
      destroy () {
        this.ue.destroy();
      },
      setContent (content) {
        this.ue.setContent(content);
      },
      getContent () {
        return this.ue.getContent();
      }
    },
    watch: {
        content (val) {
          this.ue.setContent(val);
        }
    }
  }
</script>

<style>

</style>
