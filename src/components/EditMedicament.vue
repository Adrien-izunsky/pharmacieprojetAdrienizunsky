<template>
  <div>
    <h2>Modifier Médicament</h2>
    <form @submit.prevent="updateMedicament">
      <input v-model="medicament.denomination" placeholder="Dénomination" required />
      <input v-model="medicament.formepharmaceutique" placeholder="Forme Pharmaceutique" required />
      <input v-model="medicament.qte" type="number" placeholder="Quantité" required />
      <input type="file" @change="handleFileUpload" />
      <div v-if="photo">
        <p>Nouvelle image :</p>
        <img :src="photo" alt="Prévisualisation" style="max-width: 150px;" />
      </div>
      <button type="submit">Mettre à jour</button>
    </form>
  </div>
</template>

<script setup>
import { ref, onMounted, defineProps, defineEmits } from 'vue';

const props = defineProps(['medicamentId']);
const emit = defineEmits(['updated']);
const idPharmacie = 16;

const medicament = ref({});
const photo = ref('');

const fetchMedicament = async () => {
  const response = await fetch(`https://apipharmacie.pecatte.fr/api/${idPharmacie}/medicaments/${props.medicamentId}`);
  medicament.value = await response.json();
};

onMounted(fetchMedicament);

const handleFileUpload = (event) => {
  const file = event.target.files[0];
  const reader = new FileReader();
  reader.onload = () => {
    photo.value = reader.result;
  };
  reader.readAsDataURL(file);
};

const updateMedicament = async () => {
  await fetch(`https://apipharmacie.pecatte.fr/api/${idPharmacie}/medicaments`, {
    method: 'PUT',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      id: medicament.value.id,
      denomination: medicament.value.denomination,
      formepharmaceutique: medicament.value.formepharmaceutique,
      qte: medicament.value.qte,
      photo: photo.value || medicament.value.photo
    })
  });
  emit('updated');
};
</script>
