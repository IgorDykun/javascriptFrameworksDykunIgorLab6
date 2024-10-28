<template>
  <div class="mb-3">
    <label :for="id">{{ label }}</label>
    <input :type="type" :id="id" v-model="inputValue" class="form-control" @input="emitInput" />
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, watch } from 'vue';

export default defineComponent({
  name: 'InputField',
  props: {
    id: String,
    label: String,
    modelValue: String,
    type: {
      type: String,
      default: 'text',
    },
  },
  setup(props, { emit }) {
    const inputValue = ref(props.modelValue);

    const emitInput = () => emit('update:modelValue', inputValue.value);

    watch(() => props.modelValue, (newVal) => inputValue.value = newVal);

    return { inputValue, emitInput };
  },
});
</script>
