<template>
  <div class="app-container">
    <h1>Gestion de la Pharmacie</h1>
    <nav>
      <button @click="currentView = 'list'">Voir les Médicaments</button>
      <button @click="currentView = 'add'">Ajouter un Médicament</button>
    </nav>

    <MedicamentList v-if="currentView === 'list'" @edit="editMedicament" />
    <AddMedicament v-if="currentView === 'add'" @added="currentView = 'list'" />
    <EditMedicament v-if="currentView === 'edit'" :medicamentId="selectedMedicament" @updated="currentView = 'list'" />
  </div>
</template>

<script setup>
import { ref } from 'vue';
import MedicamentList from './components/MedicamentList.vue';
import AddMedicament from './components/AddMedicament.vue';
import EditMedicament from './components/EditMedicament.vue';

const currentView = ref('list');
const selectedMedicament = ref(null);

const editMedicament = (id) => {
  selectedMedicament.value = id;
  currentView.value = 'edit';
};
</script>

<style scoped>
.app-container {
  font-family: Arial, sans-serif;
  padding: 20px;
  text-align: center;
}
nav button {
  margin: 10px;
  padding: 10px 20px;
}
</style>