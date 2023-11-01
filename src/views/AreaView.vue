<template>
  <div class="card row">
    <div class="card-content">
      <div class="col s10">
        <h3>Areas</h3>
      </div>
      <div class="col s2">
        <AreaAdd />
      </div>
    </div>
  </div>
  <div class="card row">
    <div class="card-content">
      <div class="row">
        <div class="input-field col s6">
          <i class="material-icons prefix">search</i>
          <input
            id="search"
            type="text"
            class="validate"
            v-model="searchTerm"
            @input="searchItems()"
          />
          <label for="search">Buscar</label>
        </div>
      </div>
      <table class="highlight">
        <thead>
          <tr>
            <th><span class="card-title col s3 m3">Area</span></th>
            <th><span class="card-title col s4 m4">Encargado</span></th>
            <th><span class="card-title col s2 m2">N° Funcionarios</span></th>
            <th><span class="card-title col s3 m3">Opciones</span></th>
          </tr>
        </thead>
        <tbody v-for="(item, index) in items" :key="item.id">
          <tr>
            <th>
              <p class="card-content col s3">{{ item.NombreArea }}</p>
            </th>
            <th>
              <p class="card-content col s4">{{ item.NombreEncargado }}</p>
            </th>
            <th>
              <p class="card-content col s2">{{ item.NumeroFuncionarios }}</p>
            </th>
            <th>
              <div class="card-content s1">
                <a class="btn-floating btn-large waves-effect waves-light red">
                  <i class="material-icons" @click="Delete(item.id)">delete</i>
                </a>
              </div>
              <div class="card-content s1">
                <a
                  class="btn-floating btn-large waves-effect waves-light orange btn modal-trigger"
                  @click="ModalOpen(item, index)"
                  :href="'#modal' + index"
                >
                  <i class="material-icons">edit</i>
                </a>
                <div :id="'modal' + index" class="modal">
                  <div class="modal-content">
                    <h5>Editar Area-Departamento</h5>
                    <div class="row">
                      <form class="col s12" @submit="Update(item.id)">
                        <div class="row">
                          <div class="input-field col s6">
                            <i class="material-icons prefix">work</i>
                            <input
                              id="icon_work"
                              type="text"
                              class="validate"
                              v-model="payload.NombreArea"
                            />
                            <label for="Departamento">Departamento</label>
                          </div>
                          <div class="input-field col s6">
                            <i class="material-icons prefix">account_circle</i>
                            <input
                              id="icon_prefix"
                              type="text"
                              class="validate"
                              v-model="payload.NombreEncargado"
                            />
                            <label for="Encargado">Encargado</label>
                          </div>
                          <div class="input-field col s6">
                            <i class="material-icons prefix">filter_9_plus</i>
                            <input
                              id="icon_filter_9_plus"
                              type="number"
                              class="validate"
                              v-model="payload.NumeroFuncionarios"
                            />
                            <label for="N° Funcionarios">N° Funcionarios</label>
                          </div>
                        </div>
                        <div>
                          <button
                            class="btn waves-effect waves-light right"
                            type="submit"
                          >
                            Guardar
                            <i class="material-icons right">send</i>
                          </button>
                        </div>
                      </form>

                      <button
                        class="btn waves-effect waves-light red"
                        @click="ModalCerrar(index)"
                      >
                        Cancelar
                        <i class="material-icons right">close</i>
                      </button>
                    </div>
                  </div>
                </div>
              </div>
            </th>
          </tr>
        </tbody>
      </table>
      <div class="pagination-wrapper">
        <ul class="pagination">
          <li class="disabled">
            <a href="#!" @click="paginateHandler(currentPage - 1)"
              ><i class="material-icons">chevron_left</i></a
            >
          </li>
          <li
            v-for="page in totalPages"
            :key="page"
            :class="{ active: page === currentPage }"
            class="waves-effect"
          >
            <a href="#!" @click="paginateHandler(page)">{{ page }}</a>
          </li>
          <li class="waves-effect">
            <a href="#!" @click="paginateHandler(currentPage + 1)"
              ><i class="material-icons">chevron_right</i></a
            >
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import M from "materialize-css";
import AreaAdd from "@/components/AreaAdd.vue";

export default {
  name: "AreaView",
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
      modalInstances: [],
      searchTerm: "",
      currentPage: 1,
    };
  },
  methods: {
    Delete(id) {
      if (confirm("Estas seguro de eliminar?")) {
        this.axios({
          method: "delete",
          url: this.api + "/areas/" + id,
        })
          .then(() => {
            this.getList();
          })
          .catch((error) => {
            console.error(error);
          });
      }
    },
    getList() {
      return new Promise((resolve, reject) => {
        const pageParam = `?_page=${this.currentPage}&_limit=5`;
        this.axios({
          method: "get",
          url: this.api + "/areas" + pageParam,
        })
          .then((response) => {
            this.items = response.data;
            resolve();
          })
          .catch((error) => {
            console.error(error);
            reject(error);
          });
      });
    },
    searchItems() {
      const searchParam = this.searchTerm
        ? "?q=" + encodeURIComponent(this.searchTerm)
        : "";
      this.axios({
        method: "get",
        url: this.api + "/areas" + searchParam,
      })
        .then((response) => {
          this.items = response.data;
        })
        .catch((error) => {
          console.error(error);
        });
    },
    ModalOpen(item, index) {
      M.updateTextFields();
      this.payload = {
        id: item.id,
        NombreArea: item.NombreArea,
        NombreEncargado: item.NombreEncargado,
        NumeroFuncionarios: item.NumeroFuncionarios,
      };
      if (this.modalInstances[index + 1]) {
        this.modalInstances[index + 1].open();
      }
    },
    ModalCerrar(index) {
      if (this.modalInstances[index + 1]) {
        this.modalInstances[index + 1].close();
      }
    },
    Update(id) {
      if (confirm("Estas seguro de editar?")) {
        this.axios({
          method: "patch",
          url: this.api + "/areas/" + id,
          data: this.payload,
        })
          .then(() => {
            this.getList();
          })
          .catch((error) => {
            console.error(error);
          });
      }
    },
    paginateHandler(pageNumber) {
      this.currentPage = pageNumber;
      this.getList();
    },
  },
  components: {
    AreaAdd,
  },
  computed: {
    totalPages() {
      return Math.ceil(this.items.length / 5);
    },
  },
  mounted() {
    this.getList().then(() => {
      var elems = document.querySelectorAll(".modal");
      this.modalInstances = M.Modal.init(elems);
    });
  },
};
</script>
<style>
.pagination-wrapper {
  display: flex;
  justify-content: center;
  margin-top: 20px;
}
</style>
