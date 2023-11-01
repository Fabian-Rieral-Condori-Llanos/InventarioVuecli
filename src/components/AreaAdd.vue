<template>
  <h3>
    <a
      class="waves-effect waves-light btn modal-trigger"
      @click="ModalOpen()"
      href="#agregarmodal"
      >Agregar</a
    >
  </h3>
  <div id="agregarmodal" class="modal">
    <div class="modal-content">
      <h5>Agregar Area</h5>
      <div class="row">
        <form class="col s12" @submit="nuevo()">
          <div class="row">
            <div class="input-field col s6">
              <i class="material-icons prefix">work</i>
              <input
                id="icon_work"
                type="text"
                v-model="payload.NombreArea"
                class="validate"
              />
              <label for="icon_work">Departamento</label>
            </div>
            <div class="input-field col s6">
              <i class="material-icons prefix">account_circle</i>
              <input
                id="icon_prefix"
                type="text"
                v-model="payload.NombreEncargado"
                class="validate"
              />
              <label for="icon_prefix">Encargado</label>
            </div>
            <div class="input-field col s6">
              <i class="material-icons prefix">filter_9_plus</i>
              <input
                id="icon_filter_9_plus"
                type="number"
                v-model="payload.NumeroFuncionarios"
                class="validate"
              />
              <label for="icon_filter_9_plus">N° Funcionarios</label>
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
  name: "AreaAdd",
  props: {
    msg: String,
  },
  data() {
    const api = process.env.VUE_APP_API;
    return {
      api: api,
      items: [],
      modal: null,
      payload: {
        NombreArea: null,
        NombreEncargado: null,
        NumeroFuncionarios: null,
      },
    };
  },
  methods: {
    getList() {
      this.axios({
        method: "get",
        url: this.api + "/areas",
      })
        .then((response) => {
          this.items = response.data;
        })
        .catch((error) => {
          console.error(error);
        });
    },
    nuevo() {
      this.axios({
        method: "post",
        url: this.api + "/areas",
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
      var modalElement = document.querySelector("#agregarmodal");
      var modalInstance = M.Modal.getInstance(modalElement);
      modalInstance.open();
    },
    ModalCerrar() {
      var modalElement = document.querySelector("#agregarmodal");
      var modalInstance = M.Modal.getInstance(modalElement);
      modalInstance.close();
    },
  },
  components: {},
  mounted() {
    this.getList();
    var elems = document.querySelector("#agregarmodal");
    var instances = M.Modal.init(elems);
    this.modal = instances;
  },
};
</script>
