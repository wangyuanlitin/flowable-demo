<template>
  <div class="containers">
    <div class="canvas" ref="canvas"></div>
    <button class="button" @click="setColor">显示流程</button>
  </div>
</template>

<script setup>
import { onMounted } from 'vue';
import BpmnJS from 'bpmn-js'
import MoveCanvas from 'diagram-js/lib/navigation/movecanvas';
import data from './data.js'
import 'bpmn-js/dist/assets/diagram-js.css';
import 'bpmn-js/dist/assets/bpmn-font/css/bpmn.css';
import 'bpmn-js/dist/assets/bpmn-font/css/bpmn-codes.css';
import 'bpmn-js/dist/assets/bpmn-font/css/bpmn-embedded.css';

let viewer = null

onMounted(() => {
  viewer = new BpmnJS({
    container: 'body',
    additionalModules: [
      MoveCanvas
    ]
  })

  fetch('http://127.0.0.1:5173/contractExamine.bpmn20.xml')
    .then((res) => res.text())
    .then((data) => {
      viewer.importXML(data)
    })
})

const setColor = () => {
  var canvas = viewer.get('canvas');

  // 处理开始节点
  if (data.data.length > 0) {
    canvas.addMarker('start', 'highlight');
  }
  // 已经走过的节点
  data.data.forEach(element => {
    console.log(element.taskDefinitionKey)
    canvas.addMarker(element.taskDefinitionKey, 'highlight');
  });

  // 当前节点
  canvas.addMarker('legalDepartmentDirector1Sign', 'current')
}
</script>

<style>
body {
  padding: 0;
  margin: 0;
  width: 100vw;
  height: 100vh;
}
.button {
  width: 120px;
  height: 40px;
  /* background-color: red; */
  position: fixed;
  z-index: 999999;
  right: 20px;
  top: 20px;
}
.highlight:not(.djs-connection) .djs-visual > :nth-child(1) {
  fill: green !important; /* color elements as green */
}

.current:not(.djs-connection) .djs-visual > :nth-child(1) {
  fill: yellow !important; /* color elements as green */
}
</style>