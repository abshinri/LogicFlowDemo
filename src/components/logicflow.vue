<script setup lang="ts">
import { onMounted, ref } from "vue";
import LogicFlow from "@logicflow/core";
import "@logicflow/core/dist/style/index.css";

const logicflow = ref(null);

onMounted(() => {
  const lf = new LogicFlow({
    container: logicflow.value as unknown as HTMLElement,
    grid: true,
  });
  lf.render({
    nodes: [
      {
        id: "1",
        type: "rect",
        x: 100,
        y: 100,
      },
      {
        id: "2",
        type: "circle",
        x: 300,
        y: 200,
      },
    ],
    edges: [
      {
        sourceNodeId: "1",
        targetNodeId: "2",
        type: "polyline",
      },
    ],
  });
});
</script>

<template>
  <div class="logicflow" id="logicflow" ref="logicflow"></div>
</template>

<style scoped>
.logicflow {
  margin: 0;
  padding: 0;
  width: 100vw;
  height: 100vh;
}
</style>
