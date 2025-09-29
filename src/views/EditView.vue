<template>
  <div class="row mt-3">
    <div class="col-md-6 offset-md-3">
      <div class="card">
        <div class="card-header bg-dark text-white text-center">Editar Visita</div>
        <div class="card-body">
          <form @submit="guardar">
            <!-- Hotel Name -->
            <div class="input-group mb-3">
              <span class="input-group-text"><i class="fa-solid fa-gift"></i></span>
              <input type="text" v-model="hotelName" id="hotelName" class="form-control" maxlength="50" placeholder="Hotel" required>
            </div>

            <!-- Auditor Name -->
            <div class="input-group mb-3">
              <span class="input-group-text"><i class="fa-solid fa-comment"></i></span>
              <input type="text" v-model="auditorName" id="auditorName" class="form-control" maxlength="50" placeholder="Auditor" required>
            </div>

            <!-- Scheduled Date -->
            <div class="input-group mb-3">
              <span class="input-group-text"><i class="fa-solid fa-calendar"></i></span>
              <input type="datetime-local" v-model="scheduledDate" id="scheduledDate" class="form-control" required>
            </div>

            <!-- Status -->
            <div class="input-group mb-3">
              <span class="input-group-text"><i class="fa-solid fa-info-circle"></i></span>
              <input 
                type="text" 
                v-model="status" 
                id="status" 
                class="form-control" 
                maxlength="20" 
                placeholder="Pendiente, Cancelada, Realizada"
                required
                :class="{'is-invalid': status && !isValidStatus}"
              >
              <!-- Error message -->
              <div v-if="status && !isValidStatus" class="invalid-feedback">
                El estado debe ser "Pendiente", "Cancelada" o "Realizada".
              </div>
            </div>

            <!-- Comentarios -->
            <div class="input-group mb-3">
              <span class="input-group-text"><i class="fa-solid fa-comment"></i></span>
              <input type="text" v-model="comentarios" id="comentarios" class="form-control" maxlength="150" placeholder="Comentarios" required>
            </div>

            <!-- Guardar Button -->
            <div class="d-grid col-6 mx-auto">
              <button class="btn btn-success"><i class="fa-solid fa-floppy-disk"></i> Guardar</button>
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { show_alerta, enviarSolicitud } from '../funciones';
import { useRoute } from 'vue-router';
import axios from 'axios';

export default {
  data() {
    return {
      id: 0,
      hotelName: '',
      auditorName: '',
      scheduledDate: '',
      status: '',  // El estado será un string
      comentarios: '',
      url: '/api/visitas'
    }
  },
  computed: {
    isValidStatus() {
      return ['Pendiente', 'Cancelada', 'Realizada'].includes(this.status);
    }
  },
  mounted() {
    const route = useRoute();
    this.id = route.params.id;
    this.url = `${this.url}/${this.id}`;

    this.getVisita();
  },
  methods: {
    getVisita() {
      axios.get(this.url)
        .then(res => {
          const data = res.data;
          this.hotelName = data.hotelName;
          this.auditorName = data.auditorName;
          this.scheduledDate = new Date(data.scheduledDate).toISOString().slice(0, 16); 
          this.comentarios = data.comentarios;
          this.status = data.status;  // Directamente asignamos el estado como string
        })
        .catch(err => console.error('❌ Error cargando visita:', err));
    },

    guardar(event) {
      event.preventDefault();

      // Validaciones
      if (this.hotelName.trim().length < 3)
        return show_alerta('El nombre del hotel debe tener al menos 3 caracteres', 'warning', 'hotelName');
      if (this.auditorName.trim().length < 3)
        return show_alerta('El nombre del auditor debe tener al menos 3 caracteres', 'warning', 'auditorName');
      if (this.scheduledDate === '')
        return show_alerta('Selecciona la fecha', 'warning', 'scheduledDate');
      if (!this.isValidStatus)
        return show_alerta('El estado debe ser "Pendiente", "Cancelada" o "Realizada"', 'warning', 'status');
      if (this.comentarios.trim() === '')
        return show_alerta('Escribe comentarios', 'warning', 'comentarios');

      const parametros = {
        hotelName: this.hotelName.trim(),
        auditorName: this.auditorName.trim(),
        scheduledDate: this.scheduledDate,
        status: this.status,  // Usamos el string directamente
        comentarios: this.comentarios.trim()
      };

      enviarSolicitud('PUT', parametros, this.url, 'Visita actualizada');
    }
  }
}
</script>

<style scoped>
.is-invalid {
  border-color: red;
}
.invalid-feedback {
  color: red;
}
</style>
