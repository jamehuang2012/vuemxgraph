<template>
  <div>
    <div class="graphContainer" tabindex="1" ref="container"/>
    <button @click="addProcess()">Agrega Proceso</button>
  </div>
</template>

<script>
import {
  mxGraph as MxGraph,
  mxConstants,
  mxUtils,
  mxSwimlaneManager,
  
  // mxEvent as MxEvent,
  // mxPoint as MxPoint,
  // mxRubberband as MxRubberBand,
  // mxKeyHandler as MxKeyHandler
} from "mxgraph-js";

export default {
  name: "Graph",
  data() {
    return {
      graph: null,
      parent: null
    };
  },
  mounted() {
    this.initGraph();
  },
  methods: {
    initGraph() {
      // Creates the graph inside the given container

      this.graph = new MxGraph(this.$refs.container);

      var style = this.graph.getStylesheet().getDefaultVertexStyle();
      style[mxConstants.STYLE_SHAPE] = mxConstants.SHAPE_SWIMLANE;
      style[mxConstants.STYLE_VERTICAL_ALIGN] = "middle";
      style[mxConstants.STYLE_LABEL_BACKGROUNDCOLOR] = "white";
      style[mxConstants.STYLE_FONTSIZE] = 18;
      style[mxConstants.STYLE_STARTSIZE] = 25;
      style[mxConstants.STYLE_HORIZONTAL] = false;
      style[mxConstants.STYLE_FONTCOLOR] = "black";
      style[mxConstants.STYLE_STROKECOLOR] = "black";
      style[mxConstants.STYLE_FILLCOLOR] = "lightblue";
      style[mxConstants.STYLE_LABEL_BACKGROUNDCOLOR] = "none";

      style = mxUtils.clone(style);
      style[mxConstants.STYLE_SHAPE] = mxConstants.SHAPE_RECTANGLE;
      style[mxConstants.STYLE_FONTSIZE] = 10;
      style[mxConstants.STYLE_ROUNDED] = true;
      style[mxConstants.STYLE_HORIZONTAL] = true;
      style[mxConstants.STYLE_VERTICAL_ALIGN] = "middle";
      delete style[mxConstants.STYLE_STARTSIZE];
      style[mxConstants.STYLE_LABEL_BACKGROUNDCOLOR] = "none";
      style[mxConstants.STYLE_FILLCOLOR] = "lightblue";
      this.graph.getStylesheet().putCellStyle("process", style);

      new mxSwimlaneManager(this.graph);

      // Gets the default parent for inserting new cells. This
      // is normally the first child of the root (ie. layer 0).
      this.parent = this.graph.getDefaultParent();

      // Adds cells to the model in a single step
      this.graph.getModel().beginUpdate();
      try {
        var pool1 = this.graph.insertVertex(
          this.parent,
          null,
          "Pool 1",
          0,
          0,
          800,
          0
        );
        pool1.setConnectable(false);

        var lane1a = this.graph.insertVertex(
          pool1,
          null,
          "Lane A",
          0,
          0,
          640,
          110
        );
        lane1a.setConnectable(false);

        let v1 = this.graph.insertVertex(
          this.lane1a,
          "v1",
          "Proceso 1",
          80,
          20,
          80,
          30,
          "process"
        );
        let v2 = this.graph.insertVertex(
          this.lane1a,
          "v2",
          "Proceso 2",
          200,
          20,
          80,
          30,
          "process"
        );

        this.graph.insertEdge(this.parent, null, "", v1, v2);
      } finally {
        // Updates the display
        this.graph.getModel().endUpdate();
        this.graph.setEnabled(false);
      }
    },
    addProcess() {
      // Gets the default parent for inserting new cells. This
      // is normally the first child of the root (ie. layer 0).
      this.parent = this.graph.getDefaultParent();

      // Adds cells to the model in a single step
      this.graph.getModel().beginUpdate();
      try {
        let v2 = this.graph.getModel().getCell("v2");

        let v3 = this.graph.insertVertex(
          this.parent,
          null,
          "Proceso 3",
          400,
          20,
          80,
          30,
          "process"
        );
        this.graph.insertEdge(this.parent, null, "", v2, v3);
      } finally {
        // Updates the display
        this.graph.getModel().endUpdate();
      }
    }
  }
};
</script>
<style lang="scss">
.graphEditorContainer {
  width: 100%;
  height: 100%;
  .graphContainer {
    background: #efefef;
  }
}
</style>