<template>
  <div class="relative text-lg w-48">
    <button
      class="flex items-center justify-between w-24 relative outline-none py-2 px-2 bg-white text-sm text-gray-700 placeholder-gray-400 focus:outline-none focus:shadow-outline shadow appearance-none border rounded"
      @click="isOptionsExpanded = !isOptionsExpanded"
      @blur="isOptionsExpanded = false"
      :disabled="false"
    >
      <span>{{ selectedOption }}</span>
      <svg
        fill="none"
        viewBox="0 0 24 24"
        stroke="currentColor"
        class="h-4 w-4 transform transition-transform duration-200 ease-in-out"
        :class="isOptionsExpanded ? 'rotate-180' : 'rotate-0'"
      >
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          stroke-width="2"
          d="M19 9l-7 7-7-7"
        />
      </svg>
    </button>
    <transition
      enter-active-class="transform transition duration-500 ease-custom"
      enter-class="-translate-y-1/2 scale-y-0 opacity-0"
      enter-to-class="translate-y-0 scale-y-100 opacity-100"
      leave-active-class="transform transition duration-300 ease-custom"
      leave-class="translate-y-0 scale-y-100 opacity-100"
      leave-to-class="-translate-y-1/2 scale-y-0 opacity-0"
    >
      <ul
        v-show="isOptionsExpanded"
        class="absolute left-0 right-0 mb-4 bg-white divide-y rounded-lg shadow-lg overflow-hidden"
      >
        <li
          v-for="(option, index) in options"
          :key="index"
          class="px-3 py-2 flex items-center transition-colors cursor-pointer duration-300 hover:bg-gray-200"
          @mousedown.prevent="setOption(option)"
        >
          <img :src="option.flag" alt="" class="mr-2">
          <span>{{ option.name }}</span>
        </li>
      </ul>
    </transition>
  </div>
</template>

<script>
export default {
  data () {
    return {
      isOptionsExpanded: false,
      selectedOption: this.defaultOption,
      // options: ["EUR", "USD", "CHF", "PLN", "GBP"]
      options: [
        {name: "EUR", flag: "euro.svg"},
        {name: "USD", flag: "united.svg"},
        {name: "CHF", flag: "switzerland.svg"},
        {name: "PLN", flag: "poland.svg"},
        {name: "GBP", flag: "england.svg"},
      ]
    }
  },
  props: {
    defaultOption: {
      type: String,
      required: true
    },
  },
  methods: {
    setOption (option) {
      this.selectedOption = option.name
      this.isOptionsExpanded = false
      this.$emit("clearExchange", false)
      this.$emit("selectOption", this.selectedOption)
    }
  }
}
</script>
