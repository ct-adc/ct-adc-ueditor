<template>
  <div id="demo" class="container-fluid">
    <div class="col-md-6 col-md-offset-3 mt20 mb20">
      <u-editor :content="data.content"
               :config="ue.config"
               :toolbars="ue.toolbars"
               @change="clearViewSource"
               ref="ueditor"></u-editor>
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
        <button type="button" class="btn btn-info"
                @click="showModal">
          <i class="glyphicon glyphicon-plu pr5"></i>弹窗测试
        </button>
      </div>
    </div>

    <div class="col-md-6 col-md-offset-3 mb20">
      <pre>
        {{ source }}
      </pre>
    </div>

    <modal-test ref="modal"></modal-test>
  </div>
</template>

<script type="text/ecmascript-6">
  import UEditor from '../../../../../src/component/UEditor.vue';

  import ModalTest from './ModalTest.vue';

  export default {
    components: {
      UEditor,
      ModalTest
    },
    data () {
      return {
        data: {
          content: '请输入些文字吧'
        },
        source: '',
        ue: {
          config: { // 配置项
            zIndex: 900, // 非模态框，默认值
            maximumWords: 200,
            initialFrameHeight: 150
          },
          toolbars: [] // 工具栏设置，非空则完全覆盖默认工具栏
        }
      }
    },
    methods: {
      clearUEditor () {
        this.data.content = '';
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
          error (err) {
            let miniMsg = new MiniMsg({
              content: JSON.stringify(err),
              container: $('body'),
              type: 'error',
              duration: 2
            });

            miniMsg.animation();
          }
        });
      },
      showModal () {
          this.$refs.modal.init();
      }
    }
  }
</script>

<style rel="stylesheet/scss" lang="scss">

</style>
