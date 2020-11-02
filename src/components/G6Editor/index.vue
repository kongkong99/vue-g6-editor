<template>
  <div
    id="mountNode"
    :style="{width:width}"
  >
    <div class="editor">
      <div class="bottom-container">
        <item-panel />
        <page
          :height="height"
          :width="width"
          :data="data"
          class="editor-page"
        />
      </div>
    </div>
    <Flow />
  </div>
</template>

<script>
import ItemPanel from "../ItemPanel";
import Page from "../Page";
import Flow from "../Flow"
export default {
  name: "G6Editor",
  components: {
    ItemPanel,
    Page,
    Flow
  },
  props: {
    height: {
      type: Number,
      default: document.documentElement.clientHeight
    },
    width: {
      type: Number,
      default: document.documentElement.clientWidth
    },
    data: {
      type: Object,
      default: () => {}
    }
  },
  created() {
  },
  data() {
    return {
    };
  },
  methods: {
  }
};
</script>

<style scoped>
.editor {
  position: relative;
  width: 100%;
  user-select: none;
  -moz-user-select: none;
  -webkit-user-select: none;
  -ms-user-select: none;
}
.bottom-container {
  position: relative;
}

.editor-page {
  border: 2px solid red;
}
</style>