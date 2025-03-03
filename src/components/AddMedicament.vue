<template>
  <div>
    <h2>Ajouter un Médicament</h2>
    <form @submit.prevent="addMedicament">
      <input v-model="denomination" placeholder="Dénomination" required />
      <input v-model="formepharmaceutique" placeholder="Forme Pharmaceutique" required />
      <input v-model="qte" type="number" placeholder="Quantité" required />
      <input type="file" @change="handleFileUpload" />
      <div v-if="photo">
        <p>Prévisualisation :</p>
        <img :src="photo" alt="Prévisualisation" style="max-width: 150px;" />
      </div>
      <button type="submit">Ajouter</button>
    </form>
  </div>
</template>

<script setup>
import { ref, defineEmits } from 'vue';

const emit = defineEmits(['added']);
const idPharmacie = 16;

const denomination = ref('');
const formepharmaceutique = ref('');
const qte = ref(1);
const photo = ref('');

const handleFileUpload = (event) => {
  const file = event.target.files[0];
  const reader = new FileReader();
  reader.onload = () => {
    photo.value = reader.result;
  };
  reader.readAsDataURL(file);
};

const addMedicament = async () => {
  await fetch(`https://apipharmacie.pecatte.fr/api/${idPharmacie}/medicaments`, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      denomination: denomination.value,
      formepharmaceutique: formepharmaceutique.value,
      qte: qte.value,
      photo: photo.value
    })
  });
  emit('added');
};
</script>