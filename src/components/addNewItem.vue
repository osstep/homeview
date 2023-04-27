<script setup>

import {reactive, ref} from 'vue';
const props = defineProps({
    data: Array,
    id: Number
})

const flag = ref(false)
const localData = ref(props.data);

const values = reactive({
    name: '',
    amount: '',
    spending: ''
})

const toggleFlag = () => {
    flag.value = !flag.value
}

const addItem = () => {
    localData.value.push({
        id: props.id,
        name: values.name,
        amount: values.amount,
        defaultAmount: values.amount,
        spending: values.spending, 
        edit: false
    })
    values.name = ''
    values.amount = ''
    values.spending = ''
}

</script>

<template>
    <button type="button" class="btn btn-primary" @click="toggleFlag">Добавить</button>
    <div v-if="flag">
        <input v-model="values.name" type="text" placeholder="название">
        <input v-model="values.amount" type="number" placeholder="Количество">
        <input v-model="values.spending" type="number" placeholder="Расход">
        <button type="button" class="btn btn-primary" @click="addItem">Ок</button>
        <button type="button" class="btn btn-primary" @click="toggleFlag">закрыть</button>
    </div>
</template>