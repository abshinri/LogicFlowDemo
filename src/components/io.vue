<script setup lang="ts">
import { onMounted, ref, defineProps } from "vue";
import type LogicFlow from "@logicflow/core";

type IProps = {
  lf: LogicFlow;
};

const props = defineProps<IProps>();

function download(filename: string, text: string) {
  var element = document.createElement("a");
  element.setAttribute(
    "href",
    "data:text/plain;charset=utf-8," + encodeURIComponent(text)
  );
  element.setAttribute("download", filename);

  element.style.display = "none";
  document.body.appendChild(element);

  element.click();

  document.body.removeChild(element);
}
type FileEventTarget = EventTarget & { files: FileList };

function downloadXml() {
  const data = props.lf.getGraphData() as string;
  console.log(data);
  download("logic-flow.xml", data);
}

function uploadXml(ev: any) {
  console.log(ev);
  const file = (ev.target as FileEventTarget).files[0];
  const reader = new FileReader();
  reader.onload = (event: ProgressEvent<FileReader>) => {
    console.log(event);

    if (event.target) {
      const xml = event.target.result as string;
      props.lf.render(xml);
    }
  };
  reader.readAsText(file); // you could also read images and other binaries
}

function downloadImage() {
  props.lf.getSnapshot();
}
</script>

<template>
  <div class="logicflow-io" id="logicflowIO" ref="logicflowIO">
    <span @click="downloadXml">
      <img src="../assets/img/download.png" alt="下载XML" />
    </span>
    <span id="download-img" @click="downloadImage">
      <img src="../assets/img/img.png" alt="下载图片" />
    </span>
    <span id="upload-xml">
      <input type="file" class="upload" @change="uploadXml($event)" />
      <img src="../assets/img/upload.png" alt="上传XML" />
    </span>
  </div>
</template>

<style scoped>
.logicflow-io {
  position: absolute;
  left: 10px;
  bottom: 10px;
  z-index: 9999;
  background: rgba(255, 255, 255, 0.8);
  box-shadow: 0 1px 4px rgba(0, 0, 0, 0.3);
  padding: 10px;
  display: flex;
}
.logicflow-io > span {
  margin: 0 5px;
  cursor: pointer;
}
#upload-xml {
  position: relative;
  overflow: hidden;
  display: inline-block;
  cursor: pointer;
}
.upload {
  position: absolute;
  z-index: 99;
  left: 0;
  top: 0;
  opacity: 0;
  cursor: pointer;
}
.upload::-webkit-file-upload-button {
  cursor: pointer;
}
</style>
