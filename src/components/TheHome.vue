<template>
  <div class="containers">
    <div class="canvas" ref="canvas"></div>
    <div class="button" @click="setColor"></div>
  </div>
</template>

<script setup>
import { onMounted } from 'vue';
import BpmnJS from 'bpmn-js'
import 'bpmn-js/dist/assets/diagram-js.css';
import 'bpmn-js/dist/assets/bpmn-font/css/bpmn.css';
import 'bpmn-js/dist/assets/bpmn-font/css/bpmn-codes.css';
import 'bpmn-js/dist/assets/bpmn-font/css/bpmn-embedded.css';

let viewer = null

onMounted(() => {
  viewer = new BpmnJS({
    container: 'body'
  })

  fetch('http://127.0.0.1:5173/contractExamine.bpmn20.xml')
    .then((res) => res.text())
    .then((data) => {
      viewer.importXML(data)
    })
})

const setColor = () => {
  var canvas = viewer.get('canvas');

canvas.addMarker('undertakeDepartmentDirectorSign', 'highlight');
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
  width: 60px;
  height: 40px;
  background-color: red;
  position: fixed;
  z-index: 999999;
  right: 20px;
  top: 20px;
}
.highlight:not(.djs-connection) .djs-visual > :nth-child(1) {
  fill: green !important; /* color elements as green */
}
</style>