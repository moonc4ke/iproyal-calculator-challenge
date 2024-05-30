<script setup lang="ts">
import { ref, inject, type Ref } from 'vue';
import { type HistoryItem } from '@/types/types';
// import CalculatorOperation from './CalculatorOperation.vue';

const history = inject<Ref<HistoryItem[]>>('history', ref([]));
const result = inject<Ref<number | null>>('result', ref(null));
const setInputs = inject<((n1: number, n2: number, op: string) => void) | undefined>('setInputs');
const importHistory = inject<(event: Event) => void>('importHistory');
const exportHistory = inject<() => void>('exportHistory');
const clearHistory = inject<() => void>('clearHistory');

const fileInput = ref<HTMLInputElement | null>(null);

const triggerFileInput = (): void => {
  fileInput.value?.click();
};

const handleSetOperation = (entry: HistoryItem) => {
  const parts = entry.expression.split(' ');
  const n1 = parseFloat(parts[0]);
  const op = parts[1];
  const n2 = parseFloat(parts[2]);
  result.value = entry.result;
  setInputs?.(n1, n2, op);
};
</script>

<template>
  <div>
    <!-- <CalculatorOperation /> -->
    <ul>
      <li
        v-for="(entry, index) in history"
        :key="index"
        @click="handleSetOperation(entry)"
        @keydown.enter="handleSetOperation(entry)"
      >
        {{ entry.expression }} = {{ entry.result }}
      </li>
    </ul>
    <input id="import" type="file" @change="importHistory" hidden ref="fileInput" />
    <button @click="triggerFileInput" type="button">Import</button>
    <button @click="exportHistory" type="button">Export</button>
    <button @click="clearHistory" type="button">Clear</button>
  </div>
</template>
