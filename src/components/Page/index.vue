<template>
  <div class="page">
    <div
      :id="pageId"
      class="graph-container"
      style="position: relative;"
    ></div>
  </div>
</template>


<script>
import eventBus from "@/utils/eventBus";
import G6 from "@antv/g6/build/g6";
import { initBehavors } from "@/behavior";
import Editor from "@/components/Base/Editor";
import command from "@/command";
export default {
  data() {
    return {
      pageId: "graph-container",
      graph: null,
      selectedItem: [],
      editor: {},
      command: null,
    };
  },
  props: {
    height: {
      type: Number,
      default: 0
    },
    width: {
      type: Number,
      default: 0
    },
    data: {
      type: Object,
      default: () => {}
    }
  },
  created() {

    this.bindEvent()
    initBehavors();

  },
  mounted() {
      this.init();
  },
  methods: {
    init() {
      const height =  this.height - 42 
      const width =  this.width - 400

      this.graph = new G6.Graph({
        container: "graph-container",
        height: height,
        width: width,
        modes: {
          // 支持的 behavior
          default: [
            "drag-canvas",
            "zoom-canvas",
            "hover-node",
            "select-node",
            "hover-edge",
            "keyboard",
            "customer-events",
            "add-menu"
          ],
          mulitSelect: ["mulit-select"],
          addEdge: ["add-edge"],
          moveNode:[ "drag-item"]
        }
      });
      this.editor.emit("afterAddPage", { graph: this.graph, command:this.command });
      this.readData();
    },
    readData() {
      let data = this.data;
      if (data) {
        this.graph.read(data);
      }
    },
    bindEvent() {
      this.editor = new Editor();
      this.command = new command(this.editor);
      eventBus.$on("updateItem", item => {
        this.command.executeCommand("update", [item]);
      });
      eventBus.$on("addItem", item => {
        this.command.executeCommand("add", [item]);
      });
      eventBus.$on("nodeselectchange", () => {
        this.selectedItem = this.graph.findAllByState("node", "selected");
        this.selectedItem = this.selectedItem.concat(
          ...this.graph.findAllByState("edge", "selected")
        );
      });
      eventBus.$on("deleteItem", () => {
        this.handleDelete();
      });
    },
    handleDelete() {
      if (this.selectedItem.length > 0) {
        this.command.executeCommand("delete", this.selectedItem);
        this.selectedItem = null;
      }
    }

  }
};
</script>

<style scoped>
.page {
  margin-left: 200px;
}
</style>