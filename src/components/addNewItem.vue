<script setup>

import {reactive, ref} from 'vue';
const props = defineProps({
    data: Array,
    id: Number
})

const flag = ref(false)
const localData = ref(props.data);
const localId = ref(props.id)

const values = reactive({
    name: '',
    amount: '',
    spending: ''
})

const toggleFlag = () => {
    flag.value = !flag.value
}

const addItem = () => {
    if (values.name && values.amount && values.spending) {
        localData.value.push({
            id: localId.value++,
            name: values.name,
            amount: values.amount,
            defaultAmount: values.amount,
            spending: values.spending, 
            edit: false
        })
        
        values.name = ''
        values.amount = ''
        values.spending = ''

        toggleFlag()
        
    }
}

</script>

<template>
    <button type="button" class="btn btn-primary" @click="toggleFlag">Добавить</button>
    <div class="modal__mask" v-if="flag">
        <div class="modal__wrapper">
            <h4>создание карточки</h4>
            <input v-model="values.name" type="text" placeholder="название">
            <input v-model="values.amount" type="number" placeholder="Количество">
            <input v-model="values.spending" type="number" placeholder="Расход">
            <div>
                <button type="button" class="btn btn-primary" @click="addItem">ок</button>
                <button type="button" class="btn btn-primary" @click="toggleFlag">закрыть</button>
            </div>
        </div>
    </div>
</template>
<style scoped>
    .modal__mask {
        position: fixed;
        width: 100%;
        height: 100%;
        background-color: rgba(0, 0, 0, 0.5);
        z-index: 999;
        top: 0;
        left: 0;
        display: flex;
        align-items: center;
    }
    .modal__wrapper {
        
        display: flex;
        margin: 0 auto;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        height: 400px;
        width: 300px;
        background: white;
    }
    input {
        margin-bottom: 10px;
    }
    button {
        margin-right: 10px;
        width: 90px;
    }
    h4 {
        margin-bottom: 20px;
    }
</style>