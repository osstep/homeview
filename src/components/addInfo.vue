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
    <div class="item__info" v-if="!item.edit">
      <div class="item__title">{{ item.name }}</div>
      <pieCharts :data="computedData" :item="item"></pieCharts>
      <div class="edit__menu">
        <div class="edit__wrapper__item" @click="toggleEdit(item)">&#9998;</div>
        <div class="edit__wrapper__item" @click="deleteItem(item)">&#10006;</div>
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
  
  height: 400px;
  width: 300px;
  padding: 5px;
  margin-bottom: 20px;
  border-radius: 10px;
  background: rgb(238,174,202);
  background: radial-gradient(circle, rgba(238,174,202,1) 0%, rgba(202,226,255,1) 100%);
}
.item__info {
  position: relative;
}
.item__title {
  text-align: center;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 20px;
  padding-top: 20px;
  height: 80px;
  text-transform: uppercase;
  
}
.item__modal {
  height: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;


}
.edit__menu {
  display: flex;
  justify-content: end;
  position: absolute;
  top: 0;
  right: 0;
}
.edit__wrapper__item {
  margin-right: 5px;
  cursor: pointer;
}
button {
  margin: 5px;
  
}

</style>