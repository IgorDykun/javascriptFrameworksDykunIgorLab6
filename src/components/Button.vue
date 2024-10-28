<template>
  <button @click="$emit('click')" class="btn" :class="btnClass">{{ label }}</button>
</template>

<script lang="ts">
import { defineComponent, PropType } from 'vue';

export default defineComponent({
  name: 'Button',
  props: {
    label: String,
    btnClass: {
      type: String as PropType<'btn-primary' | 'btn-danger' | 'btn-success'>,
      default: 'btn-primary',
    },
  },
});
</script>
