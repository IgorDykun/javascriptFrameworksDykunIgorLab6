<template>
  <div>
    <SearchBar @filter-by-name="filterParticipants" />
    <table class="table table-striped">
      <thead>
        <tr>
          <th @click="sortByName">Name <i class="icon-sort"></i></th>
          <th @click="toggleSortByDob">Date of Birth <i class="icon-sort"></i></th>
          <th>Email</th>
          <th>Phone</th>
          <th>Edit</th>
          <th>Delete</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(participant, index) in filteredParticipants" :key="index">
          <td>{{ participant.name }}</td>
          <td>{{ participant.dob }}</td>
          <td>{{ participant.email }}</td>
          <td>{{ participant.phone }}</td>
          <td>
            <Button label="Edit" @click="openEditModal(participant)" />
          </td>
          <td>
            <Button label="Delete" btnClass="btn-danger" @click="openDeleteModal(participant)" />
          </td>
        </tr>
      </tbody>
    </table>
    <Modal v-if="showEditModal" @close="closeEditModal">
      <!-- Add your edit form here -->
      <form @submit.prevent="updateParticipant">
        <input v-model="participantToEdit.name" placeholder="Name" />
        <input v-model="participantToEdit.dob" placeholder="Date of Birth" />
        <input v-model="participantToEdit.email" placeholder="Email" />
        <input v-model="participantToEdit.phone" placeholder="Phone" />
        <button type="submit">Save</button>
      </form>
    </Modal>
    <Modal v-if="showDeleteModal" @close="closeDeleteModal">
      <p>Are you sure you want to delete this participant?</p>
      <button @click="deleteParticipant">Yes, Delete</button>
      <button @click="closeDeleteModal">Cancel</button>
    </Modal>
  </div>
</template>

<script lang="ts">
import { defineComponent, PropType, ref } from 'vue';
import Button from './Button.vue';
import Modal from './Modal.vue';
import SearchBar from './SearchBar.vue';

interface Participant {
  name: string;
  dob: string;
  email: string;
  phone: string;
}

export default defineComponent({
  name: 'ParticipantsTable',
  components: { Button, Modal, SearchBar },
  props: {
    participants: {
      type: Array as PropType<Participant[]>,
      required: true,
    },
  },
  setup(props) {
    const showEditModal = ref(false);
    const showDeleteModal = ref(false);
    const filteredParticipants = ref<Participant[]>([...props.participants]);
    const participantToEdit = ref<Participant | null>(null);
    const participantToDelete = ref<Participant | null>(null);
    const isDobAscending = ref(true); // Додано для зберігання напрямку сортування

    const sortByName = () => {
      filteredParticipants.value.sort((a, b) => a.name.localeCompare(b.name));
    };

    const toggleSortByDob = () => {
      // Змінюємо напрямок сортування
      isDobAscending.value = !isDobAscending.value;

      filteredParticipants.value.sort((a, b) => {
        const dateA = new Date(a.dob).getTime();
        const dateB = new Date(b.dob).getTime();
        return isDobAscending.value ? dateA - dateB : dateB - dateA; // Сортуємо за зростанням або спаданням
      });
    };

    const filterParticipants = (query: string) => {
      filteredParticipants.value = props.participants.filter(p => p.name.includes(query));
    };

    const openEditModal = (participant: Participant) => {
      participantToEdit.value = { ...participant }; // Створюємо новий об'єкт для редагування
      showEditModal.value = true;
    };

    const closeEditModal = () => {
      participantToEdit.value = null;
      showEditModal.value = false;
    };

    const updateParticipant = () => {
      if (participantToEdit.value) {
        const index = props.participants.findIndex(p => p.email === participantToEdit.value!.email);
        if (index !== -1) {
          props.participants[index] = { ...participantToEdit.value }; // Оновлюємо учасника
          localStorage.setItem('participants', JSON.stringify(props.participants)); // Оновлюємо localStorage
        }
      }
      closeEditModal();
    };

    const openDeleteModal = (participant: Participant) => {
      participantToDelete.value = participant;
      showDeleteModal.value = true;
    };

    const closeDeleteModal = () => {
      participantToDelete.value = null;
      showDeleteModal.value = false;
    };

    const deleteParticipant = () => {
      if (participantToDelete.value) {
        const index = props.participants.findIndex(p => p.email === participantToDelete.value!.email);
        if (index !== -1) {
          props.participants.splice(index, 1); // Видаляємо учасника
          localStorage.setItem('participants', JSON.stringify(props.participants)); // Оновлюємо localStorage
        }
      }
      closeDeleteModal();
    };

    return {
      filteredParticipants,
      showEditModal,
      showDeleteModal,
      participantToEdit,
      participantToDelete,
      sortByName,
      toggleSortByDob,
      filterParticipants,
      openEditModal,
      closeEditModal,
      updateParticipant,
      openDeleteModal,
      closeDeleteModal,
      deleteParticipant,
    };
  },
});
</script>
