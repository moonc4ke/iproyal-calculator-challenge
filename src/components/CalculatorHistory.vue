<script setup lang="ts">
import { ref, inject, type Ref } from 'vue';
import { type HistoryItem } from '@/types/types';
import CalculatorOperation from './CalculatorOperation.vue';
import CalculatorButton from './CalculatorButton.vue';

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

const handleImportHistory = (event: Event) => {
  importHistory?.(event);
  if (fileInput.value) {
    fileInput.value.value = '';
  }
};
</script>

<template>
  <div>
    <CalculatorOperation />

    <section class="calculator-history-section">
      <h2>History:</h2>
      <ul v-if="history.length > 0" class="calculator-history-section-list">
        <li
          v-for="(entry, index) in history"
          :key="index"
          @click="handleSetOperation(entry)"
          @keydown.enter="handleSetOperation(entry)"
        >
          {{ entry.expression }} = {{ entry.result }}
        </li>
      </ul>

      <input id="import" type="file" @change="handleImportHistory" hidden ref="fileInput" />
      <div class="calculator-history-section-actions">
        <CalculatorButton title="Import" @click="triggerFileInput" />
        <CalculatorButton title="Export" @click="exportHistory" />
        <CalculatorButton title="Clear" @click="clearHistory" />
      </div>
    </section>
  </div>
</template>

<style scoped>
.calculator-history-section h2 {
  font-weight: 400;
  margin-bottom: 5px;
}

.calculator-history-section-list {
  margin-top: 8px;
  background-color: #5A5757;
  border-radius: 10px;
  box-shadow: 5px 5px 40px 0px #0B0B0B4D;
  padding: 10px;
}

.calculator-history-section-list li {
  list-style: none;
  cursor: pointer;
  margin-bottom: 10px;
}

.calculator-history-section-list li:hover {
  text-decoration: underline;
}

.calculator-history-section-actions {
  margin-top: 10px;
  display: flex;
  gap: 10px;
}
</style>
