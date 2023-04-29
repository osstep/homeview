<script setup>

import {ref, reactive, onMounted, watch, computed} from 'vue'
import pieCharts from './pieCharts.vue';

const props = defineProps({
    data: Object
})


const defaultValues =  reactive({
  spendingValue: '',
  amoutValue: '',
  addAmount: '',
  nameValue: '',
  defaultAmount: ''

})
const computedData = computed(()=> {
  return props.data
})
const toggleEdit = (item) => {
  item.edit = item.edit ? false : true
}

const deleteItem = (item) => {
  props.data.forEach((element, index) => {
    if (item == element) {
      props.data.splice(index, 1)
    }
  });
}
const setInfo = (item) => {
  //установить отсток
  if (defaultValues.amoutValue) {
    item.amount = +defaultValues.amoutValue;
    defaultValues.amoutValue = '';
    
  } // установить общее количество 
  if (defaultValues.defaultAmount) {
    item.defaultAmount = +defaultValues.defaultAmount;
    defaultValues.defaultAmount = '';
    
  } // ежедневный расход
  if (defaultValues.spendingValue) {
    item.spending = +defaultValues.spendingValue;
    defaultValues.spendingValue = '';
  }
  // добавление количества в остаток
  if (defaultValues.addAmount) {
      if(item.amount + +defaultValues.addAmount > item.defaultAmount) {
        item.amount += +defaultValues.addAmount;
        item.defaultAmount = item.amount
        defaultValues.addAmount = ''

      } else {
        item.amount += +defaultValues.addAmount;
        defaultValues.addAmount = ''

      }
  }
  //смена названия 
  if (defaultValues.nameValue) {
    item.name = defaultValues.nameValue;
    defaultValues.nameValue = ''
  }
  toggleEdit(item)
}
</script>


<template>
<div class="wrapper">

  <div class="wrapper__item" v-for="item in computedData" :key="item.id">
    <div v-if="!item.edit">
      {{ item.name }} <br>остаток {{ item.amount }} <br>расход {{ item.spending }} <br>начальное значение: {{ item.defaultAmount }}
      <pieCharts :data="computedData" :item="item"></pieCharts>
      <div>
        <button type="button" class="btn btn-primary" @click="toggleEdit(item)">Редактировать</button>
        <button type="button" class="btn btn-primary" @click="deleteItem(item)">Удалить</button>
      </div>
    </div>
              
    <div class="item__modal" v-if="item.edit">
      <input v-model="defaultValues.defaultAmount" type="number" placeholder="общее количество">
      <input v-model="defaultValues.amoutValue" type="number" placeholder="установить остаток">
      <input v-model="defaultValues.spendingValue" type="number" placeholder="расход в день">
      <input v-model="defaultValues.addAmount" type="number" placeholder="добавить к остатку">
      <input v-model="defaultValues.nameValue" type="text" placeholder="Новое название">
      <div>
        <button type="button" class="btn btn-primary" @click="setInfo(item)">ОК</button>
        <button type="button" class="btn btn-primary" @click="toggleEdit(item)">Закрыть</button>
      </div>
    </div>
  </div>
</div>
</template>


<style scoped>
.wrapper {
  display: flex;
  justify-content: space-around;
  margin-top: 20px;
  flex-wrap: wrap;
}
.wrapper__item {
  
  height: 450px;
  width: 300px;
  padding: 5px;
  margin-bottom: 20px;
  border-radius: 10px;
  background: rgb(135, 194, 196);
}
.item__modal {
  height: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;


}
button {
  margin: 5px;
  
}

</style>