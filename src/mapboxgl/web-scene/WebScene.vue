<template>
  <div class="sm-widget-web-scene">
    <div :id="sceneId" class="sm-widget-web-scene__wrap"></div>
  </div>
</template>

<script>
import WebSceneViewModel from './WebSceneViewModel';

export default {
  name: 'SmWebScene',
  props: {
    sceneUrl: {
      type: String,
      required: true
    },
    scanEffect: {
      type: Object
    },
    navigation: {
      type: Boolean,
      default: true
    }
  },
  data() {
    return {
      tooltipIsVisable: true
    };
  },
  computed: {
    sceneId() {
      return 'scene' + parseInt(Math.random() * 100);
    }
  },
  mounted() {
    this.createdScene();
  },
  beforeDestroy() {
    this.sceneViewer = null;
  },
  methods: {
    // 创建场景
    createdScene() {
      this.sceneViewModel = new WebSceneViewModel(
        this.sceneId,
        this.sceneUrl,
        this.scanEffect,
        this.navigation
      );
      this.sceneViewModel.on('createsceneviewersucceeded', () => {
        this.sceneViewer = this.sceneViewModel.sceneViewer;
      });
      this.sceneViewModel.on('sceneisprivate', () => {
        this.$message.destroy();
        this.$message.error('当前服务并未公开');
      });
    }
  }
};
</script>