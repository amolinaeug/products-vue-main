<template>
  <div class="row mt-3">
    <div class="col-md-6 offset-md-3">
      <div class="card">
        <div class="card-header bg-dark text-white text-center">Crear Visita</div>
        <div class="card-body">
          <form @submit="guardar">
            <!-- Hotel Name -->
            <div class="input-group mb-3">
              <span class="input-group-text"><i class="fa-solid fa-gift"></i></span>
              <input type="text" v-model="hotelName" class="form-control" maxlength="50" placeholder="Hotel" required>
            </div>

            <!-- Auditor Name -->
            <div class="input-group mb-3">
              <span class="input-group-text"><i class="fa-solid fa-user"></i></span>
              <input type="text" v-model="auditorName" class="form-control" maxlength="50" placeholder="Auditor" required>
            </div>

            <!-- Scheduled Date -->
            <div class="input-group mb-3">
              <span class="input-group-text"><i class="fa-solid fa-calendar"></i></span>
              <input type="datetime-local" v-model="scheduledDate" class="form-control" required>
            </div>

            <!-- Status -->
            <div class="input-group mb-3">
              <span class="input-group-text"><i class="fa-solid fa-info-circle"></i></span>
              <select v-model="status" class="form-control" required>
                <option disabled value="">Selecciona un estado</option>
                <option value="Pendiente">Pendiente</option>
                <option value="Realizada">Realizada</option>
                <option value="Cancelada">Cancelada</option>
              </select>
            </div>

            <!-- Comentarios -->
            <div class="input-group mb-3">
              <span class="input-group-text"><i class="fa-solid fa-comment"></i></span>
              <input type="text" v-model="comentarios" class="form-control" maxlength="150" placeholder="Comentarios" required>
            </div>

            <!-- Botón Guardar -->
            <div class="d-grid col-6 mx-auto">
              <button class="btn btn-success">
                <i class="fa-solid fa-floppy-disk"></i> Guardar
              </button>
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { show_alerta, enviarSolicitud } from '../funciones';

export default {
  data() {
    return {
      hotelName: '',
      auditorName: '',
      scheduledDate: '',
      status: '',         
      comentarios: '',
      url: '/api/visitas'
    };
  },

  methods: {
    guardar(event) {
      event.preventDefault(); 

      // Validaciones
      if (this.hotelName.trim().length < 3)
        return show_alerta('El nombre del hotel debe tener al menos 3 caracteres', 'warning', 'hotelName');
      if (this.auditorName.trim().length < 3)
        return show_alerta('El nombre del auditor debe tener al menos 3 caracteres', 'warning', 'auditorName');
      if (!this.scheduledDate)
        return show_alerta('Selecciona la fecha', 'warning', 'scheduledDate');
      if (!this.status)
        return show_alerta('Selecciona un estado', 'warning', 'status');
      if (this.comentarios.trim() === '')
        return show_alerta('Escribe comentarios', 'warning', 'comentarios');

      const parametros = {
        hotelName: this.hotelName.trim(),
        auditorName: this.auditorName.trim(),
        scheduledDate: this.scheduledDate,
        status: this.status,
        comentarios: this.comentarios.trim()
      };
      enviarSolicitud('POST', parametros, this.url, 'Visita creada');
    }
  }
};
</script>
