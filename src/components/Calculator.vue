<template>
  <!-- Main Div for the whole page -->
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
  // allows passing data from parent components to this component
  props: {
    msg: String,
  },
  data() {
    return {
      // The current value displayed on the calculator screen
      calcVal: '' as string,
      // An array containing calculator button values
      calcBtns: ['C', '%', '=', '+', 7, 8, 9, '-', 4, 5, 6, '*', 1, 2, 3, '/', 0, '.'] as (string)[],
      // The selected operator (+, -, *, or /) for performing calculations
      operators: null as string | null,
      // The previous value stored before selecting an operator
      prevCalcVal: '' as string,
    };
  },
  methods: {
    // Function to handle button clicks and perform actions
    action(button: string) {
      // If the clicked button is a number or decimal point
      if (!isNaN(Number(button)) || button === '.') {
        // Append the button value to the current calculator value
        this.calcVal += button;
      }

      // If the clicked button is the clear button ('C')
      if (button === 'C') {
        // Clear the calculator value to reset the input
        this.calcVal = '';
      }

      // If the clicked button is the percentage button ('%')
      if (button === '%') {
        // Convert the value to a percentage
        this.calcVal = (Number(this.calcVal) / 100).toString();
      }

      // If the clicked button is an operator
      if (['/', '+', '-', '*'].includes(button)) {
        // Store the selected operator
        this.operators = button;
        // Save the current value as the previous value
        this.prevCalcVal = this.calcVal;
        // Clear the calculator value for the next input
        this.calcVal = '';
      }

      // If the clicked button is the equals button ('=')
      if (button === '=') {
        // Evaluate the expression and update the calculator value
        this.calcVal = eval(this.prevCalcVal + this.operators + this.calcVal).toString();
        // Reset the previous value
        this.prevCalcVal = '';
        // Reset the selected operator
        this.operators = null;
      }
    },
  },
});
</script>


<!-- STYLES -->
<style scoped>
.bg-gray-900 {
  background: rgb(77, 0, 77);
  border: 2px solid rgb(77, 0, 77);
  border-radius: 5px;
}

.hover\:bg-blue-800 {
  background: lightcoral;
}

.bg-blue-900 {
  background: lightcoral;
}


.hover\:bg-blue-800:hover{
  cursor: pointer;
  background: rgb(77, 0, 77);
}

.bg-green-600{
  background: turquoise;
  color: black;
}


</style>


