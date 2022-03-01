<template>
  <div id="app" class="flex w-screen min-h-screen p-4 bg-blue-500 ">
    <calculator />
  </div>
</template>

<script>
  import calculator from './components/calculator.vue'

  export default {
    components: {
      calculator
    }
  }
</script>
