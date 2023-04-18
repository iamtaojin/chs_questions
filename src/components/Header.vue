<script setup>
import { ref } from 'vue'
const props = defineProps([
  'setSetting'
])
const setting = ref({
  from:0,
  n:20,
  rand:false
})

const set = () =>{

}

</script>


<template>
  <span>开始：<input type="number" v-model="setting.from"></span>
  <span>题数：<input type="number" v-model="setting.n">题</span>
  <span>乱序？<input type="checkbox" v-model="setting.rand"></span>
  <span><button @click="set">OK</button></span>
</template>

<style scoped>
#header {
  position: absolute;
  display: flex;
  flex-direction: row;
  top: 0;
  left: 0;
  margin: 0;
  background-color: #232323;
  width: 100%;
  text-align: center;
  /* justify-content:space-between; */
  align-items: center;
}
input{
  color:black;
}
input{
  display: inline;
}
</style>
