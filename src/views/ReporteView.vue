<template>
  <div>
    <h3>Revisa la consola para ver tus visitas</h3>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'EmptyComponent',
  data() {
    return {
      visitas: []
    };
  },
  mounted() {
    axios.get('/api/visitas')
      .then(response => {
        this.visitas = response.data._embedded?.visitas || response.data; // depende de tu JSON
        console.log('Mis visitas:', this.visitas); // 🔥 Aquí ves el JSON
      })
      .catch(error => {
        console.error('Error al obtener visitas:', error);
      });
  }
};
</script>

<style scoped>
/* Opcional */
</style>
