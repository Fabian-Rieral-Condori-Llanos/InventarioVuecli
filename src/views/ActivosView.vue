<template>
  <div class="card row">
    <div class="card-content">
      <div class="col s10">
        <h3>Activos</h3>
      </div>
      <div class="col s2">
        <ActivosAdd />
      </div>
    </div>
  </div>
  <div class="card row">
    <div class="card-content">
      <div class="row">
        <div class="input-field col s8">
          <i class="material-icons prefix">search</i>
          <input
            id="search"
            type="text"
            class="validate"
            v-model="searchTerm"
            @input="getList()"
          />
          <label for="search">Buscar</label>
        </div>
        <div class="input-field col s4">
          <select @change="filter('Estado', $event.target.value)">
            <option value="" selected>Todos</option>
            <option value="nuevo">Nuevo</option>
            <option value="usado">Usado</option>
            <option value="en desuso">En desuso</option>
          </select>

          <label>Filtros</label>
        </div>
      </div>
      <table class="highlight">
        <thead>
          <tr>
            <th><span class="card-title col s3 m3">Modelo</span></th>
            <th><span class="card-title col s4 m4">Marca</span></th>
            <th><span class="card-title col s2 m2">Estado</span></th>
            <th><span class="card-title col s3 m3">Area</span></th>
            <th><span class="card-title col s3 m3">Opciones</span></th>
          </tr>
        </thead>
        <tbody v-for="(item, index) in items" :key="item.id">
          <tr>
            <th>
              <p class="card-content col s3">{{ item.name }}</p>
            </th>
            <th>
              <p class="card-content col s4">{{ item.Marca }}</p>
            </th>
            <th>
              <p class="card-content col s2">{{ item.Estado }}</p>
            </th>
            <th>
              <p class="card-content col s2">{{ item.AreaId }}</p>
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
                    <h5>Editar Activo</h5>
                    <div class="row">
                      <form class="col s12" @submit="Update(item.id)">
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
                              <option
                                :value="area.id"
                                v-for="area in areas"
                                :key="area.id"
                              >
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
import ActivosAdd from "@/components/ActivosAdd.vue";

export default {
  name: "AreaView",
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
      modalInstances: [],
      searchTerm: "",
      currentPage: 1,
      filtroEstado: "",
    };
  },
  methods: {
    filter(name, value) {
      this.filtroEstado = value === "" ? "" : "&" + name + "=" + value;
      this.getList();
    },
    Delete(id) {
      if (confirm("Estas seguro de eliminar?")) {
        this.axios({
          method: "delete",
          url: this.api + "/activos/" + id,
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
        const pageParam = `&_page=${this.currentPage}&_limit=5`;
        const url =
          this.api +
          "/activos/?q=" +
          this.searchTerm +
          this.filtroEstado +
          pageParam;
        this.axios({
          method: "get",
          url: url,
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
    ModalOpen(item, index) {
      M.updateTextFields();
      this.payload = {
        id: item.id,
        name: item.name,
        Marca: item.Marca,
        Estado: item.Estado,
        AreaId: item.AreaId,
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
          url: this.api + "/activos/" + id,
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
    ActivosAdd,
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

      var selectElems = document.querySelectorAll("select");
      this.selectInstances = M.FormSelect.init(selectElems);

      this.getListArea();
    });
  },
  watch: {
    filtroEstado: {
      handler() {
        this.getList();
      },
    },
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
