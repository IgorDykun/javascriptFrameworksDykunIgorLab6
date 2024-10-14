<template>
  <div class="container mt-4">
    <h2>Participants</h2>
    <table class="table table-striped">
      <thead>
        <tr>
          <th>#</th>
          <th>Name</th>
          <th>Date of Birth</th>
          <th>Email</th>
          <th>Phone number</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(participant, index) in participants" :key="index">
          <td>{{ index + 1 }}</td>
          <td>{{ participant.name }}</td>
          <td>{{ participant.dob }}</td>
          <td>{{ participant.email }}</td>
          <td>{{ participant.phone }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script lang="ts">
import { defineComponent, PropType } from 'vue';

interface Participant {
  name: string;
  dob: string;
  email: string;
  phone: string;
}

export default defineComponent({
  name: 'ParticipantsTable',
  props: {
    participants: {
      type: Array as PropType<Participant[]>,
      required: true,
    },
  },
});
</script>
