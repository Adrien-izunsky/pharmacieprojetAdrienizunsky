<template>
  <div>
    <button @click="goToAdd">Ajouter un Médicament</button>
    <MedicamentList />
  </div>
</template>

<script setup>
import { useRouter } from "vue-router";
import MedicamentList from "../components/MedicamentList.vue";

const router = useRouter();
const goToAdd = () => {
  router.push("/add");
};
</script>
