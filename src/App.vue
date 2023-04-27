<script setup>
import {ref, reactive, onMounted, watchEffect} from 'vue'
import axios from 'axios'
import addInfo from './components/addInfo.vue';
import addNewItem from './components/addNewItem.vue';
import pieCharts from './components/pieCharts.vue';
let id = 0;


const data = reactive([
  {id: id++, name: 'Мусорные пакеты', amount: 20, defaultAmount: 30, spending: 1, edit: false},
  {id: id++, name: 'Таблетки для посудомойки', amount: 30, defaultAmount: 30, spending: 2, edit: false},
  {id: id++, name: 'Шампунь', amount: 30, defaultAmount: 30, spending: 1, edit: false},
  {id: id++, name: 'Гель для душа', amount: 30, defaultAmount: 30, spending: 1, edit: false}
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
