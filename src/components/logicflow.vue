<script setup lang="ts">
import Io from "./io.vue";
import Pattern from "./pattern.vue";

import { onMounted, ref } from "vue";
import LogicFlow from "@logicflow/core";
import {
  BpmnElement,
  BpmnAdapter,
  BpmnXmlAdapter,
  Snapshot,
  Control,
  Menu,
  SelectionSelect,
} from "@logicflow/extension";
import "@logicflow/core/dist/style/index.css";
import "@logicflow/extension/lib/style/index.css";

LogicFlow.use(BpmnElement);
LogicFlow.use(BpmnAdapter);
LogicFlow.use(BpmnXmlAdapter);
LogicFlow.use(Snapshot);
LogicFlow.use(Control);
LogicFlow.use(Menu);
LogicFlow.use(SelectionSelect);
const logicflowRef = ref<any>(null);

const lf = ref<any>(null);

onMounted(() => {
  lf.value = new LogicFlow({
    container: logicflowRef.value as unknown as HTMLElement,
    grid: {
      size: 10,
      visible: true,
      type: "mesh",
      config: {
        color: "#ababab",
        thickness: 1,
      },
    },
    keyboard: {
      enabled: true,
    },
  });

  lf.value.render();
});
</script>

<template>
  <div>
    <div class="logicflow" id="logicflow" ref="logicflowRef"></div>
    <Pattern :lf="lf"></Pattern>
    <Io :lf="lf"></Io>
  </div>
</template>

<style scoped>
.logicflow {
  margin: 0;
  padding: 0;
  width: 100vw;
  height: 100vh;
}
</style>
