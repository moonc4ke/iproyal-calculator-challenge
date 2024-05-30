<script setup lang="ts">
import { inject, type Ref } from 'vue';
// import CalculatorHistory from './CalculatorHistory.vue';

const result = inject<Ref<number | null>>('result');
const calculate = inject<() => void>('calculate');
const errorMessage = inject<Ref<string>>('errorMessage');
</script>

<template>
  <div>
    <!-- <CalculatorHistory /> -->
    <button @click="calculate" type="button">Calculate</button>
    <p v-if="errorMessage">{{ errorMessage }}</p>
    <p>Result: {{ result }}</p>
  </div>
</template>
