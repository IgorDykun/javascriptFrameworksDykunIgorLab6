<template>
  <div class="container mt-4">
    <form @submit.prevent="saveParticipant" class="p-4 border rounded shadow">
      <div class="mb-3">
        <label for="name" class="form-label">Name</label>
        <input type="text" v-model="name" class="form-control" required />
      </div>
      <div class="mb-3">
        <label for="dob" class="form-label">Date of Birth</label>
        <input type="date" v-model="dob" class="form-control" required :max="maxDate" />
      </div>
      <div class="mb-3">
        <label for="email" class="form-label">Email</label>
        <input type="email" v-model="email" class="form-control" required />
      </div>
      <div class="mb-3">
        <label for="phone" class="form-label">Phone number</label>
        <input type="tel" v-model="phone" class="form-control" required @input="checkPhone"/>
        <div v-if="!isPhoneValid && phoneTouched" class="text-danger mt-1">Invalid phone number</div>
      </div>
      <button type="submit" class="btn btn-primary w-100">Save</button>
    </form>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, computed } from 'vue';

interface Participant {
  name: string;
  dob: string;
  email: string;
  phone: string;
}

export default defineComponent({
  name: 'RegistrationForm',
  setup(_, { emit }) {
    const name = ref('');
    const dob = ref('');
    const email = ref('');
    const phone = ref('');
    const maxDate = new Date().toISOString().split('T')[0];
    const phoneTouched = ref(false);

    const isPhoneValid = computed(() => {
      const phoneRegex = /^[+]?\d{10,14}$/;
      return phoneRegex.test(phone.value);
    });

    const checkPhone = () => {
      phoneTouched.value = true;
    };

    const saveParticipant = () => {
      phoneTouched.value = true;

      if (new Date(dob.value) > new Date(maxDate)) {
        alert('Date of birth cannot be in the future');
        return;
      }

      if (!isPhoneValid.value) {
        alert('Invalid phone number');
        return;
      }

      const newParticipant: Participant = {
        name: name.value,
        dob: dob.value,
        email: email.value,
        phone: phone.value,
      };

      emit('add-participant', newParticipant);

      name.value = '';
      dob.value = '';
      email.value = '';
      phone.value = '';
      phoneTouched.value = false;
    };

    return { name, dob, email, phone, maxDate, saveParticipant, isPhoneValid, checkPhone, phoneTouched };
  },
});
</script>
