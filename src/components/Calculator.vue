<template>
  <!-- Calculator container -->
  <div class="p-3 max-w-400px mx-auto bg-gray-900" style="margin: 50px auto;">

    <!-- Calculator Result Display -->
    <!-- Display the current calculated value or 0 if empty -->
    <div class="w-full rounded m-1 p-3 text-right text-white font-bold bg-blue-900">
      {{ calcVal || 0 }}
    </div>  

    <!-- Calculator Buttons -->
    <!-- Create a grid layout for the calculator buttons -->
    <div class="grid grid-cols-4 gap-1">
      <!-- Loop through each button in calcBtns array -->
      <div v-for="button in calcBtns" :key="button">
        <!-- Calculator button element -->
        <div 
          class="text-white text-center m-1 py-3 bg-blue-900 rounded hover:bg-blue-800"
          :class="{'bg-green-600': ['C', '*', '/', '+', '-', '=', '%'].includes(button)}"
          @click="action(String(button))"
        >
          <!-- Display the button label -->
          {{ button }}
        </div>
      </div>
    </div>
  </div>    
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  // Component name
  name: 'Calculator',
  // Props passed to the component
  props: {
    msg: String,
  },
  // Component data
  data() {
    return {
      // Current calculated value shown on the display
      calcVal: '' as string,
      // Array of calculator buttons with labels and operators
      calcBtns: ['C', '%', '=', '+', 7, 8, 9, '-', 4, 5, 6, '*', 1, 2, 3, '/', 0, '.'] as (string)[],
      // Operator selected by the user (+, -, *, /)
      operators: null as string | null,
      // Previous calculated value before an operator is selected
      prevCalcVal: '' as string,
    };
  },
  // Component methods
  methods: {
    // Function to handle button clicks
    action(button: string) {
      // If the button is a number or a decimal point
      if (!isNaN(Number(button)) || button === '.') {
        // Add the button label to the current calculated value
        this.calcVal += button;
      }

      // If the button is 'C' (clear)
      if (button === 'C') {
        // Reset the current calculated value to empty
        this.calcVal = '';
      }

      // If the button is '%' (percentage)
      if (button === '%') {
        // Convert the current calculated value to a percentage
        this.calcVal = (Number(this.calcVal) / 100).toString();
      }

      // If the button is an operator (+, -, *, /)
      if (['/', '+', '-', '*'].includes(button)) {
        // Store the selected operator
        this.operators = button;
        // Save the current calculated value as the previous one
        this.prevCalcVal = this.calcVal;
        // Reset the current calculated value to empty
        this.calcVal = '';
      }

      // If the button is '=' (equals)
      if (button === '=') {
        // Evaluate the expression using eval and update the current calculated value
        this.calcVal = eval(this.prevCalcVal + this.operators + this.calcVal).toString();
        // Reset the previous calculated value and the selected operator
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



