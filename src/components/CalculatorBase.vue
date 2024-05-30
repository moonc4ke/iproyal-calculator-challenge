<script setup lang="ts">
import { ref, provide } from 'vue';
import { type HistoryItem } from '@/types/types';
import CalculatorResult from './CalculatorResult.vue';

const num1 = ref<number>(0);
const num2 = ref<number>(0);
const operation = ref<string>('+');
const result = ref<number | null>(null);
const history = ref<HistoryItem[]>([]);
const errorMessage = ref<string>('');

const operations: { [key: string]: (a: number, b: number) => number } = {
  '+': (a, b) => a + b,
  '-': (a, b) => a - b,
  '*': (a, b) => a * b,
  '/': (a, b) => a / b,
};

const calculate = () => {
  errorMessage.value = '';

  if (operation.value === '/' && num2.value === 0) {
    errorMessage.value = 'Division by zero is not allowed.';
    return;
  }

  const operationFunc = operations[operation.value];
  if (operationFunc) {
    const calcResult = operationFunc(num1.value, num2.value);
    if (!Number.isNaN(calcResult)) {
      const roundedResult = parseFloat(calcResult.toFixed(4));
      result.value = roundedResult;
      history.value.push({ expression: `${num1.value} ${operation.value} ${num2.value}`, result: roundedResult });
    } else {
      errorMessage.value = 'Invalid calculation';
    }
  }
};

const setInputs = (n1: number, n2: number, op: string) => {
  num1.value = n1;
  num2.value = n2;
  operation.value = op;
};

const exportHistory = (): void => {
  const dataStr = JSON.stringify(history.value);
  const blob = new Blob([dataStr], { type: 'text/json' });
  const url = window.URL.createObjectURL(blob);
  const link = document.createElement('a');
  link.download = 'history.json';
  link.href = url;
  link.click();
};

const importHistory = (event: Event): void => {
  const input = event.target as HTMLInputElement;
  if (input.files && input.files.length > 0) {
    const file = input.files[0];
    const reader = new FileReader();
    reader.onload = (e) => {
      if (e.target && typeof e.target.result === 'string') {
        try {
          const importedData = JSON.parse(e.target.result);
          const validEntries: HistoryItem[] = [];

          importedData.forEach((entry: any, index: number) => {
            try {
              if (
                entry
                && typeof entry.expression === 'string'
                && typeof entry.result === 'number'
              ) {
                validEntries.push(entry);
              } else {
                // eslint-disable-next-line no-console
                console.warn(`Skipping invalid entry at index ${index}: Invalid types`);
              }
            } catch (error) {
              // eslint-disable-next-line no-console
              console.warn(`Skipping invalid entry at index ${index}:`, error);
            }
          });

          history.value = [...history.value, ...validEntries];
        } catch (error) {
          // eslint-disable-next-line no-console
          console.error('Failed to import history:', error);
        }
      }
    };
    reader.readAsText(file);
  }
};

const clearHistory = (): void => {
  history.value = [];
  operation.value = '+';
  num1.value = 0;
  num2.value = 0;
  result.value = null;
};

provide('num1', num1);
provide('num2', num2);
provide('operation', operation);
provide('result', result);
provide('calculate', calculate);
provide('setInputs', setInputs);
provide('history', history);
provide('errorMessage', errorMessage);
provide('exportHistory', exportHistory);
provide('importHistory', importHistory);
provide('clearHistory', clearHistory);
</script>

<template>
  <div>
    <CalculatorResult />
  </div>
</template>
