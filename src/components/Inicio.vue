<template>
  <div class="row">
    <div class="col s6 m6">
      <div class="card blue-grey darken-1">
        <div class="card-content white-text">
          <span class="card-title">Agregar Area - Departamento</span>
          <AreaAdd />
        </div>
      </div>
    </div>
    <div class="col s6 m6">
      <div class="card blue-grey darken-1">
        <div class="card-content white-text">
          <span class="card-title">Agregar Activos</span>
          <ActivosAdd />
        </div>
      </div>
    </div>
    <div class="col s6 m6">
      <div class="card blue-grey darken-1">
        <router-link to="/area">
          <div class="card-content white-text">
            <span class="card-title">Listar Area - Departamento</span>
          </div>
        </router-link>
      </div>
    </div>
    <div class="col s6 m6">
      <div class="card blue-grey darken-1">
        <router-link to="/activos">
          <div class="card-content white-text">
            <span class="card-title">Listar Activos</span>
          </div>
        </router-link>
      </div>
    </div>
  </div>
</template>
<script>
import AreaAdd from "./AreaAdd.vue";
import ActivosAdd from "./ActivosAdd.vue";
export default {
  name: "InicioVue",
  components: {
    AreaAdd,
    ActivosAdd,
  },
};
</script>
