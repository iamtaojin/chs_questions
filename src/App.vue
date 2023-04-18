<script setup>
import { ref } from 'vue'
import Questions from './components/Questions.vue';

</script>

<template>
  <div class="page">
    <Questions />
  </div>
</template>

<style scoped>
.page {
  display: flex;
  flex-direction: column;
  width:800px;
  padding:50px;
}
</style>
