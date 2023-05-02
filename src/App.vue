<script setup>
import {ref, reactive, onMounted, watchEffect} from 'vue'
import axios from 'axios'
import addInfo from './components/addInfo.vue';
import addNewItem from './components/addNewItem.vue';
import pieCharts from './components/pieCharts.vue';
let id = ref(0);


const data = reactive([
  {id: id.value++, name: 'Мусорные пакеты', amount: 20, defaultAmount: 30, spending: 1, edit: false},
  {id: id.value++, name: 'Таблетки для посудомойки', amount: 30, defaultAmount: 30, spending: 2, edit: false},
  {id: id.value++, name: 'Яблоки', amount: 5, defaultAmount: 10, spending: 1, edit: false},
  {id: id.value++, name: 'Помидоры', amount: 7, defaultAmount: 10, spending: 1, edit: false}
])

setInterval(() => {
  data.forEach((item) => {
    if(item.amount - item.spending >= 0) {
      item.amount = item.amount - item.spending
    }
  })
}, 5000)


</script>

<template>
  <div class="container">
    
    <addNewItem :data="data" :id="id" />
    <addInfo :data="data"> 
      <pieCharts :data="data"/>
    </addInfo>
  
  </div>
</template>

<style scoped>

</style>
