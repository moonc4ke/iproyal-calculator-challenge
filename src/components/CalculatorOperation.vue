<script setup lang="ts">
import { ref, inject, type Ref } from 'vue';

const setInputs = inject<(n1: number, n2: number, op: string) => void>('setInputs');

const num1 = inject<Ref<string>>('num1', ref('0'));
const num2 = inject<Ref<string>>('num2', ref('0'));
const operation = inject<Ref<string>>('operation', ref('+'));

const updateValues = () => {
  const num1Value = parseFloat(num1.value);
  const num2Value = parseFloat(num2.value);

  if (!Number.isNaN(num1Value) && !Number.isNaN(num2Value)) {
    setInputs?.(num1Value, num2Value, operation.value);
  }
};

</script>

<template>
  <div>
    <label for="num1">Number 1:</label>
    <input id="num1" v-model="num1" type="number" placeholder="Number 1" @change="updateValues">

    <label for="num2">Number 2:</label>
    <input id="num2" v-model="num2" type="number" placeholder="Number 2" @change="updateValues">

    <label for="operation">Operation:</label>
    <select id="operation" v-model="operation" @change="updateValues">
      <option value="+">+</option>
      <option value="-">-</option>
      <option value="*">*</option>
      <option value="/">/</option>
    </select>
  </div>
</template>
