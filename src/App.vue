<template>
  <div id="app">
    <WinnersBlock :participants="participants" />
    <RegistrationForm @add-participant="addParticipant" />
    <ParticipantsTable :participants="participants" />
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import WinnersBlock from './components/WinnersBlock.vue';
import RegistrationForm from './components/RegistrationForm.vue';
import ParticipantsTable from './components/ParticipantsTable.vue';

interface Participant {
  name: string;
  dob: string;
  email: string;
  phone: string;
}

export default defineComponent({
  name: 'App',
  components: {
    WinnersBlock,
    RegistrationForm,
    ParticipantsTable,
  },
  setup() {
    const participants = ref<Participant[]>([]);

    const addParticipant = (newParticipant: Participant) => {
      participants.value.push(newParticipant);
    };

    return { participants, addParticipant };
  },
});
</script>
