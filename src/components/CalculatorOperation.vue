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
  <div class="calculator-operation-section">
    <div class="calculator-operation-section-block">
      <label for="num1">Number 1:</label>
      <input id="num1" v-model="num1" type="number" placeholder="Number 1" @change="updateValues">
    </div>

    <div class="calculator-operation-section-block">
      <label for="operation">Operation:</label>
      <select id="operation" v-model="operation" @change="updateValues">
        <option value="+">+</option>
        <option value="-">-</option>
        <option value="*">*</option>
        <option value="/">/</option>
      </select>
    </div>

    <div class="calculator-operation-section-block">
      <label for="num2">Number 2:</label>
      <input id="num2" v-model="num2" type="number" placeholder="Number 2" @change="updateValues">
    </div>
  </div>
</template>

<style scoped>
input { outline: none; }

.calculator-operation-section label {
  font-weight: 400;
  margin-right: 5px;
}

.calculator-operation-section input, select {
  border: none;
  outline: none;
  background: none;
  padding: 8px;
  margin: 0;
  background-color: #5A5757;
  color: #fff;
  box-shadow: 5px 5px 40px 0px #0B0B0B4D;
  border-radius: 10px;
}

.calculator-operation-section input {
  -webkit-appearance: none;
  -moz-appearance: none;
  appearance: none;
  width: 50%;
  height: 20px;
}

.calculator-operation-section-block {
  margin-bottom: 10px;
}
</style>
