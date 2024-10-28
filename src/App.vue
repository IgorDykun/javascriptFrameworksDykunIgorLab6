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
  const participants = ref<Participant[]>(JSON.parse(localStorage.getItem('participants') || '[]'));

  const addParticipant = (newParticipant: Participant) => {
    if (participants.value.some(p => p.email === newParticipant.email)) {
      alert('This email is already registered.');
      return;
    }
    participants.value.push(newParticipant);
    localStorage.setItem('participants', JSON.stringify(participants.value));
  };

  return { participants, addParticipant };
}
  },
);
</script>
