<template>
  <div class="map-loader">
    <slot></slot>
  </div>
</template>
<script>
import LMap from "./LMap.js";
export default {
  name: "jqMap",
  mounted() {
    this.initMapClass();
  },
  methods: {
    initMapClass() {
      const map = window.$$jqloadMap;
      if (!map) {
        // 如果没有地图去执行地图加载器
        new LMap({
          url: "https://api.map.baidu.com/getscript",
          version: "3.0",
          key: "9kXK8VjQdjC7KUnKdpSoEbfpVcEe2RHo",
          lib: {
            url: "https://api.map.baidu.com/library/",
            version: "1.2"
          }
        });
        this.loadmap();
      } else {
        this.loadSuccess();
      }
    },
    async loadmap() {
      await window.$$jqloadMap().then(({ BMap }) => {
        window.$$Global_MAP = BMap;
        this.loadSuccess();
      });
    },

    loadSuccess() {
      this.$emit("success");
    }
  }
};
</script>
<style lang="less" scoped>
.map-loader {
  width: 100%;
  height: 100%;
  > div {
    width: 100%;
    height: 100%;
  }
}
</style>
