<script setup>
  import { reactive, ref, watch, computed, defineProps } from 'vue'
  import { Chart as ChartJS, ArcElement, Tooltip, Legend } from 'chart.js'
  import { Doughnut } from 'vue-chartjs'
  ChartJS.register(ArcElement, Tooltip, Legend)

  const props = defineProps({
    item: Object,
    data: Object
  })

  const state = reactive({
    amount: Math.round(props.item.amount / props.item.defaultAmount * 100),
    defaultAmount: props.item.defaultAmount
  })

  const computedChartData = computed(() => {
    const amount = Math.round(props.item.amount / props.item.defaultAmount * 100)
    const defaultAmount = state.defaultAmount
    return {
      datasets: [
        {
          cutout: '70%',
          data: [100 - amount, amount],
          backgroundColor: ['rgb(255, 255, 255)', 'rgb(54, 162, 235)'],
          hoverOffset: 4
        }
      ]
    }
  })

  const chartOptions = reactive({
    responsive: true,
    maintainAspectRatio: false
  })

  watch(() => props.item.amount, (newAmount) => {
    state.amount = Math.round(newAmount / props.item.defaultAmount * 100)
    chartData.datasets[0].data[0] = Math.round(newAmount / props.item.defaultAmount * 100)
    chartData.datasets[0].data[1] = 100 - Math.round(newAmount / props.item.defaultAmount * 100)
  })

  const chartData = reactive({
    datasets: [
      {
        cutout: '70%',
        data: [state.amount, 100 - state.amount],
        backgroundColor: ['rgb(255, 255, 255)', 'rgb(54, 162, 235)'],
        hoverOffset: 4
      }
    ]
  })
</script>

<template>
  <div class="pie">
    <Doughnut
      :data="computedChartData"
      :options="chartOptions"
    />
    <div class="amount">{{ state.amount }}%</div>
  </div>
</template>


<style scoped>
  .pie {
    width: 250px;
    position: relative;
    margin: 0 auto;
  }

  .amount {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translateX(-50%);
  }
</style>