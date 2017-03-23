<template>
  <div id="demo" class="container-fluid">
    <div class="col-md-6 col-md-offset-3 mt20 mb20">
      <ueditor :content="data.content"
               :config="ue.config"
               :toolbars="ue.toolbars"
               @change="clearViewSource"
               ref="ueditor"></ueditor>
    </div>

    <div class="col-md-6 col-md-offset-3 mb20">
      <div class="btn-group">
        <button type="button" class="btn btn-success"
                @click="getContent">
          <i class="glyphicon glyphicon-share pr5"></i>获得内容
        </button>
        <button type="button" class="btn btn-primary"
                @click="setContent">
          <abbr title="Need run dev"><i class="glyphicon glyphicon-pencil pr5"></i>写入内容</abbr>
        </button>
        <button type="button" class="btn btn-danger"
                @click="clearUEditor">
          <i class="glyphicon glyphicon-trash pr5"></i>清空
        </button>
      </div>
    </div>

    <div class="col-md-6 col-md-offset-3 mb20">
      <pre>
        {{ source }}
      </pre>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import UEditor from '../../../../../src/component/UEditor.vue';

  export default {
    components: {
      ueditor: UEditor
    },
    data () {
      return {
        data: {
          content: '请输入些文字吧'
        },
        source: '',
        ue: {
          config: { // 配置项
            maximumWords: 200,
            initialFrameHeight: 150
          },
          toolbars: [] // 工具栏设置，非空则完全覆盖默认工具栏
        }
      }
    },
    methods: {
      clearUEditor () {
        this.$refs.ueditor.reset();
      },
      getContent () {
        this.source = this.$refs.ueditor.getContent();
      },
      clearViewSource (content) {
        this.source = '';
      },
      setContent () {
          let _this = this;

        $.ajax({
          type: 'GET',
          url: '/api/getInfo',
          data: {},
          success (res) {
            if (res.status) {
              Object.assign(_this.data, res.data)
            } else {
              let miniMsg = new MiniMsg({
                content: res.message,
                container: $('body'),
                type: 'error',
                duration: 2
              });

              miniMsg.animation();
            }
          },
          error (error) {
            let miniMsg = new MiniMsg({
              content: JSON.stringify(error),
              container: $('body'),
              type: 'error',
              duration: 2
            });

            miniMsg.animation();
          }
        });
      }
    }
  }
</script>

<style rel="stylesheet/scss" lang="scss">

</style>
