<template>
  <div class="p-3 max-w-400px mx-auto bg-gray-900" style="margin: 50px auto;">

    <!-- Calculator Result -->
    <div class="w-full rounded m-1 p-3 text-right text-white font-bold bg-blue-900">
      {{ calcVal || 0}}
    </div>  

    <!-- Calculator Buttons -->
    <div class="grid grid-cols-4 gap-1">
      <div v-for="button in calcBtns" :key="button">
        <div 
          class="text-white text-center m-1 py-3 bg-blue-900 rounded hover:bg-blue-800"
          :class="{'bg-green-600': ['C', '*', '/', '+', '-', '=', '%'].includes(button)}"
          @click="action(String(button))"
        >
          {{ button }}
        </div>
      </div>
    </div>
  </div>    
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'Calculator',
  props: {
    msg: String,
  },
  data() {
    return {
      calcVal: '' as string,
      calcBtns: ['C', '%', '=', '+', 7, 8, 9, '-', 4, 5, 6, '*', 1, 2, 3, '/', 0, '.'] as (string)[],
      operators: null as string | null,
      prevCalcVal: '' as string,
    };
  },
  methods: {
    action(button: string) {
      if (!isNaN(Number(button)) || button === '.') {
        this.calcVal += button;
      }

      if (button === 'C') {
        this.calcVal = '';
      }

      if (button === '%') {
        this.calcVal = (Number(this.calcVal) / 100).toString();
      }

      if (['/', '+', '-', '*'].includes(button)) {
        this.operators = button;
        this.prevCalcVal = this.calcVal;
        this.calcVal = '';
      }

      if (button === '=') {
        this.calcVal = eval(this.prevCalcVal + this.operators + this.calcVal).toString();
        this.prevCalcVal = '';
        this.operators = null;
      }
    },
  },
});
</script>

<style scoped>
.bg-gray-900 {
  background: #31475e;
}

.hover\:bg-blue-800:hover{
  cursor: pointer;
  background: #3D5875;
}

.bg-green-600{
  background: #3fb984;
}
</style>


