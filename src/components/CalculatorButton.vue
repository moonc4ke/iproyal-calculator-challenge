<template>
  <div class="button" @click="handleClick" @keydown.enter="handleClick">
    {{ title }}
  </div>
</template>

<script setup lang="ts">
import { defineProps, defineEmits } from 'vue';

defineProps<{
  title: string;
}>();

const emit = defineEmits<{
  (e: 'click'): void;
}>();

const handleClick = () => {
  emit('click');
};
</script>

<style scoped>
.button {
  cursor: pointer;
  background-color: #5A5757;
  color: #fff;
  box-shadow: 5px 5px 40px 0px #0B0B0B4D;
  border-radius: 10px;
  display: flex;
  padding: 10px;
  align-items: center;
}
</style>
