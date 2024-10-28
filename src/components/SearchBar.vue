<template>
  <input v-model="searchQuery" placeholder="Search by name" @input="emitSearch" class="form-control mb-3"/>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';

export default defineComponent({
  name: 'SearchBar',
  setup(_, { emit }) {
    const searchQuery = ref('');

    const emitSearch = () => emit('filter-by-name', searchQuery.value);

    return { searchQuery, emitSearch };
  },
});
</script>
