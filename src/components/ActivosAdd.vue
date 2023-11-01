<template>
  <h3>
    <a
      class="waves-effect waves-light btn modal-trigger"
      @click="ModalOpen()"
      href="#Activomodal"
      >Agregar</a
    >
  </h3>
  <div id="Activomodal" class="modal">
    <div class="modal-content">
      <h5>Agregar Activo</h5>
      <div class="row">
        <form class="col s12" @submit="nuevo()">
          <div class="row">
            <div class="input-field col s6">
              <i class="material-icons prefix">devices_other</i>
              <input
                id="icon_work"
                type="text"
                v-model="payload.name"
                class="validate"
              />
              <label for="icon_work">Modelo</label>
            </div>
            <div class="input-field col s6">
              <i class="material-icons prefix">assignment</i>
              <input
                id="icon_prefix"
                type="text"
                v-model="payload.Marca"
                class="validate"
              />
              <label for="icon_prefix">Marca</label>
            </div>
            <div class="input-field col s6">
              <i class="material-icons prefix">format_shapes</i>
              <select v-model="payload.AreaId">
                <option :value="area.id" v-for="area in areas" :key="area.id">
                  {{ area.NombreArea }}
                </option>
              </select>
              <label for="icon_filter_9_plus">Area</label>
            </div>
            <div class="input-field col s6">
              <i class="material-icons prefix">explicit</i>
              <select v-model="payload.Estado">
                <option value="nuevo">Nuevo</option>
                <option value="usado">Usado</option>
                <option value="en desuso">En desuso</option>
              </select>
              <label for="icon_filter_9_plus">Estado</label>
            </div>
          </div>
          <div>
            <button class="btn waves-effect waves-light right" type="submit">
              Agregar
              <i class="material-icons right">send</i>
            </button>
          </div>
        </form>

        <button class="btn waves-effect waves-light red" @click="ModalCerrar()">
          Cancelar
          <i class="material-icons right">close</i>
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import M from "materialize-css";

export default {
  name: "ActivosAdd",
  props: {
    msg: String,
  },
  data() {
    const api = process.env.VUE_APP_API;
    return {
      api: api,
      items: [],
      areas: [],
      modal: null,
      payload: {
        name: null,
        Marca: null,
        Estado: null,
        AreaId: null,
      },
    };
  },
  methods: {
    getList() {
      this.axios({
        method: "get",
        url: this.api + "/activos",
      })
        .then((response) => {
          this.items = response.data;
        })
        .catch((error) => {
          console.error(error);
        });
    },
    getListArea() {
      this.axios({
        method: "get",
        url: this.api + "/areas",
      })
        .then((response) => {
          this.areas = response.data;
        })
        .catch((error) => {
          console.error(error);
        });
    },
    nuevo() {
      this.axios({
        method: "post",
        url: this.api + "/activos",
        data: this.payload,
      })
        .then((response) => {
          this.getList();
          console.log(response);
        })
        .catch((error) => {
          console.log(error);
        });
    },
    ModalOpen() {
      var modalElement = document.querySelector("#Activomodal");
      var modalInstance = M.Modal.getInstance(modalElement);
      modalInstance.open();
    },
    ModalCerrar() {
      var modalElement = document.querySelector("#Activomodal");
      var modalInstance = M.Modal.getInstance(modalElement);
      modalInstance.close();
    },
  },
  components: {},
  mounted() {
    var elems = document.querySelector("#Activomodal");
    var instances = M.Modal.init(elems);
    this.modal = instances;
    var selectElems = document.querySelectorAll("select");
    this.selectInstances = M.FormSelect.init(selectElems);
    this.getListArea();
    this.getList();
  },
};
</script>
