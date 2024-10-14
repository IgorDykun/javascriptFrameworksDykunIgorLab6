<template>
  <div class="container mt-4">
    <h2>Winners</h2>
    <ul class="list-group mb-3">
      <li class="list-group-item d-flex justify-content-between align-items-center" v-for="winner in winners" :key="winner.name">
        {{ winner.name }}
        <button @click="removeWinner(winner)" class="btn btn-danger btn-sm">x</button>
      </li>
    </ul>
    <button @click="newWinner" class="btn btn-success w-100" :disabled="winners.length >= 3 || !participants.length">New winner</button>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';

interface Participant {
  name: string;
  dob: string;
  email: string;
  phone: string;
}

export default defineComponent({
  name: 'WinnersBlock',
  props: {
    participants: {
      type: Array as () => Participant[],
      required: true,
    },
  },
  setup(props) {
    const winners = ref<Participant[]>([]);

    const newWinner = () => {
      if (props.participants.length > 0) {
        const winner = props.participants[Math.floor(Math.random() * props.participants.length)];
        if (!winners.value.includes(winner)) {
          winners.value.push(winner);
        }
      }
    };

    const removeWinner = (winner: Participant) => {
      winners.value = winners.value.filter(w => w !== winner);
    };

    return { winners, newWinner, removeWinner };
  },
});
</script>

<style scoped>
button:disabled {
  background-color: grey;
}
</style>
