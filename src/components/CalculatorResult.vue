<script setup lang="ts">
import { inject, type Ref } from 'vue';
import CalculatorHistory from './CalculatorHistory.vue';
import CalculatorButton from './CalculatorButton.vue';

const result = inject<Ref<number | null>>('result');
const calculate = inject<() => void>('calculate');
const errorMessage = inject<Ref<string>>('errorMessage');
</script>

<template>
  <div>
    <CalculatorHistory />

    <section class="calculator-result-section">
      <p class="error-message" v-if="errorMessage">{{ errorMessage }}</p>
      <div class="calculator-result-section-container">
        <CalculatorButton title="Calculate" @click="calculate" />
        <div class="calculator-result-section-container-display">
          <span class="calculator-result-section-container-display-equal">=</span>
          <span>{{ result }}</span>
        </div>
      </div>
    </section>
  </div>
</template>

<style scoped>
.calculator-result-section {
  margin-top: 40px;
}

.calculator-result-section .error-message {
  color: #C15D60;
}

.calculator-result-section-container {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.calculator-result-section-container-display {
  display: flex;
  justify-content: space-between;
  padding: 10px;
  min-width: 5rem;
  background-color: #5A5757;
  box-shadow: 5px 5px 60px 0px #0B0B0B1A;
  border-radius: 10px;
}

.calculator-result-section-container-display-equal {
  margin-right: 4px;
}
</style>
