<template>
  <div>
    <span class="logo-text fs-2 fw-bold px-3">Limansky</span>
  </div>
  <div class="container">
    <h1 class="text-center my-4">Lista de Módulos</h1>
    <div v-if="modules.length" class="table-responsive shadow-table">
        <table class="table table-striped table-bordered">
          <thead>
            <tr>
              <th class="text-center align-middle">ID</th>
              <th class="text-center align-middle">Nombre</th>
              <th class="text-center align-middle">Descripción</th>
              <th class="text-center align-middle">Fecha de entrada</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(module) in modules" :key="module.idModule">
              <td class="align-middle">{{ module.idModule }}</td>
              <td class="align-middle">{{ module.name }}</td>
              <td class="align-middle">{{ module.description}}</td>
              <td class="align-middle">{{ formatDate(module.entryDate) }}</td>
            </tr>
          </tbody>
        </table>
    </div>
  </div>
</template>

<script setup>
  import { ref, onMounted } from 'vue'

  const url = "https://api-evaluaciones-test.limansky.com/modules/modules"; 
  const modules = ref([]);
  
  const formatDate = (dateString) => {
    const date = new Date(dateString);
    const year = date.getFullYear();
    const month = date.toLocaleString('es', { month: 'long' });
    const day = date.getDate();
    const hours = date.getHours().toString().padStart(2, '0');
    const minutes = date.getMinutes().toString().padStart(2, '0');
    return `${day} de ${month} de ${year}, ${hours}:${minutes}`;
  };

  const getData = async () => {
    try {
      const response = await fetch(url);
      const data = await response.json();
      modules.value = data;
    } catch (error) {
      console.error('Error en la solicitud:', error);
    }
  };

  onMounted(() => {
    getData();
  });
</script>

<style>
  @import url('https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css');

  .shadow-table {
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
  }

  .logo-text {
      color: #2A2F7E; 
    } 
</style>
