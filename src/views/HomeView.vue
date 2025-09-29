<template>
  <div class="row">
    <div class="col-lg-8 offset-lg-2">
      <div class="table-responsive">
        <table class="table table-bordered table-hover">
          <thead>
            <tr>
              <th>#</th>
              <th>HOTEL</th>
              <th>AUDITOR</th>
              <th>COMENTARIOS</th>
              <th>FECHA</th>
              <th>STATUS</th>
              <th>ACCIONES</th>
            </tr>
          </thead>

          <tbody class="table-group-divider">
            <tr v-for="(visit, i) in visitas" :key="visit.id">
              <td>{{ i + 1 }}</td>
              <td>{{ visit.hotelName }}</td>
              <td>{{ visit.auditorName }}</td>
              <td>{{ visit.comentarios }}</td>
              <td>{{ visit.status }}</td>
              <td>{{ new Date(visit.scheduledDate).toLocaleDateString('es-MX') }}</td>
              <td>
                <router-link :to="{ path: 'edit/' + visit.id }" class="btn btn-warning">
                  <i class="fa-solid fa-edit"></i>
                </router-link> &nbsp;
                
              </td>
            </tr>
          </tbody>

        </table>
      </div>
    </div>
  </div>
</template>

<script>
  import axios from 'axios';
  import { confirmar } from '../funciones';

  export default {
    data() {
      return {
         visitas: []
      }
    },
    mounted() {
      this.getVisitas();
    },
    methods: {
      getVisitas() {
  axios.get('/api/visitas')
    .then(response => {
      this.visitas = response.data._embedded.visitas.map(v => {
        const url = v._links.self.href;
        const id = url.substring(url.lastIndexOf('/') + 1);
        return { ...v, id };
      });
    })
    .catch(error => {
      console.error("Error al obtener visitas:", error);
    });
}

    }
  }
</script>
//new Date(visit.scheduledDate).toLocaleDateString('es-MX')