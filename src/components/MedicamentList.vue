<template>
  <div>
    <h2>Liste des Médicaments</h2>

    <!-- Champ de recherche -->
    <input type="text" v-model="searchQuery" placeholder="Rechercher un médicament..." />

    <ul>
      <li v-for="med in filteredMedicaments" :key="med.id">
        <p>{{ med.denomination }} - {{ med.qte }} unités</p>
        <img v-if="med.photo" :src="`https://apipharmacie.pecatte.fr/images/${med.photo}`" alt="Photo médicament" style="max-width: 100px;" />
        <div>
          <button @click="$emit('edit', med.id)">Modifier</button>
          <button @click="deleteMedicament(med.id)">Supprimer</button>
          <button @click="incrementQte(med.id)">+1</button>
          <button @click="decrementQte(med.id)">-1</button>
        </div>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue';

const medicaments = ref([]);
const searchQuery = ref(""); // Ajout du champ de recherche
const idPharmacie = 16;

// Récupérer les médicaments depuis l'API
const fetchMedicaments = async () => {
  const response = await fetch(`https://apipharmacie.pecatte.fr/api/${idPharmacie}/medicaments`);
  medicaments.value = await response.json();
};

onMounted(fetchMedicaments);

// Ajout du filtre en temps réel
const filteredMedicaments = computed(() => {
  return medicaments.value.filter(med =>
    med.denomination.toLowerCase().includes(searchQuery.value.toLowerCase())
  );
});

const deleteMedicament = async (id) => {
  await fetch(`https://apipharmacie.pecatte.fr/api/${idPharmacie}/medicaments/${id}`, { method: 'DELETE' });
  fetchMedicaments();
};

const incrementQte = async (id) => {
  const med = medicaments.value.find(m => m.id === id);
  if (med) {
    await fetch(`https://apipharmacie.pecatte.fr/api/${idPharmacie}/medicaments`, {
      method: 'PUT',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ id: id, qte: med.qte + 1 })
    });
    fetchMedicaments();
  }
};

const decrementQte = async (id) => {
  const med = medicaments.value.find(m => m.id === id);
  if (med && med.qte > 0) {
    await fetch(`https://apipharmacie.pecatte.fr/api/${idPharmacie}/medicaments`, {
      method: 'PUT',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ id: id, qte: med.qte - 1 })
    });
    fetchMedicaments();
  }
};
</script>

<style scoped>
/* Ajout du style pour le champ de recherche */
input[type="text"] {
  width: 100%;
  padding: 10px;
  margin-bottom: 15px;
  border: 1px solid #ccc;
  border-radius: 5px;
}
</style>
