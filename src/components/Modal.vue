<template>
  <div class="modal-overlay" @click.self="$emit('close')">
    <div class="modal-content">
      <slot></slot>
      <button @click="$emit('close')" class="btn btn-secondary mt-3">Close</button>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'Modal',
});
</script>

<style scoped>
.modal-overlay { /* CSS для накладки модального вікна */ }
.modal-content { /* CSS для контенту вікна */ }
</style>
