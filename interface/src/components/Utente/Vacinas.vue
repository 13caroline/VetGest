<template>
  <div>
    <v-card flat color="#fafafa">
      <v-row>
        <v-col cols="12">
          <v-row align="center" class="my-5">
            <v-col>
              <h3 class="font-weight-regular text-uppercase">
                <v-icon small>fas fa-syringe</v-icon>
                Vacinas e desparasitações
              </h3>
            </v-col>
            <v-col
              cols="auto"
              class="ml-auto pl-0"
              v-if="this.$store.state.tipo == 'Veterinario'"
            >
              <NovaVacina
                :animal="animal"
                @clicked="imunizacao_adicionada"
              ></NovaVacina>
            </v-col>
            <v-col
              cols="auto"
              class="pl-0"
              v-if="this.$store.state.tipo == 'Veterinario'"
            >
              <NovaDesparasitacao
                :dados="animal.id"
                @clicked="imunizacao_adicionada"
              ></NovaDesparasitacao>
            </v-col>
          </v-row>

          <v-data-table
            :headers="headers"
            :items="items"
            class="elevation-1"
            hide-default-footer
            :sort-by.sync="sortBy"
      :sort-desc.sync="sortDesc"
            no-data-text="Não existem vacinas ou desparasitações registadas."
            no-results-text="Não foram encontrados resultados."
            :page.sync="page"
            :items-per-page="itemsPerPage"
            @page-count="pageCount = $event"
          >
            <template v-slot:[`item.estado`]="{ item }">
              <v-chip v-if="!passouTempo(item.data) && item.estado=='Atualizada'" color="#EF9A9A" small>
                Atrasada
              </v-chip>
              <v-chip v-else :color="estadopedido(item.estado)" small>
                {{ item.estado }}
              </v-chip>
            </template>

            <template v-slot:[`item.veterinario`]="{ item }">
              <span v-if="item.veterinario">{{ item.veterinario.nome }}</span>
            </template>

            <template v-slot:[`item.data`]="{ item }">
              {{ format(item.data) }}
            </template>

            <template v-slot:[`item.data_toma`]="{ item }">
              {{ format(item.data_toma) }}
            </template>

            <template v-slot:[`item.confirmar`]="{ item }">
              <v-icon v-if="item.estado == 'Administrada'" small color="green">
                fas fa-check-circle
              </v-icon>

              <div
                v-if="
                  (item.estado == 'Atualizada' ||
                    item.estado == 'Atrasada' ||
                    item.estado == 'Agendada') &&
                  $store.state.tipo == 'Veterinario'
                "
              >
                <ConfirmaDespar
                  :dataPrev="item.data"
                  :id="item.id"
                  @clicked="imunizacao_adicionada"
                ></ConfirmaDespar>
              </div>
            </template>
          </v-data-table>
          <div class="text-center pt-2">
            <v-pagination
              v-model="page"
              :length="pageCount"
              circle
              :total-visible="7"
              color="#2596be"
              class="custom"
            ></v-pagination>
          </div>
        </v-col>
      </v-row>
    </v-card>
  </div>
</template>

<script>
import axios from "axios";
import ConfirmaDespar from "@/components/Dialogs/ConfirmaDesparasitacao.vue";
import NovaVacina from "@/components/Dialogs/NovaVacina.vue";
import NovaDesparasitacao from "@/components/Dialogs/NovaDesparasitacao.vue";
import moment from "moment";
import store from "@/store.js";
export default {
  props: ["animal"],
  data: () => ({
    dataPrev: {},
    page: 1,
    pageCount: 0,
    itemsPerPage: 8,
    done: false,
    sortBy: "data",
    sortDesc: true,
    headers: [
      {
        text: "DATA PREVISTA",
        align: "start",
        sortable: true,
        value: "data",
      },
      {
        text: "DATA DE TOMA",
        value: "data_toma",
        sortable: true,
        align: "start",
      },
      {
        text: "TIPO",
        value: "tipo",
        sortable: true,
        align: "start",
      },
      {
        text: "VACINA CONTRA",
        value: "vacina",
        sortable: true,
        align: "start",
      },
      {
        text: "TRATAMENTO UTILIZADO",
        value: "tratamento",
        sortable: true,
        align: "start",
      },
      {
        text: "MÉDICO VETERINÁRIO",
        value: "veterinario",
        sortable: false,
        align: "start",
      },
      {
        text: "ESTADO",
        value: "estado",
        sortable: true,
        align: "center",
      },
      {
        text: "AÇÕES",
        value: "confirmar",
        sortable: false,
        align: "center",
      },
    ],
    items: [],
  }),
  components: {
    ConfirmaDespar,
    NovaVacina,
    NovaDesparasitacao,
  },
  methods: {
    passouTempo: function(pedido){
      console.log(pedido)
      return moment(pedido).isAfter();  
    }, 
    close() {
      // if medico
      this.$router.push("/medico/utente");
    },
    estadopedido(estado) {
      if (estado == "Administrada") return "#9AE5FF";
      else if (estado == "Atrasada") return "#EF9A9A";
      else return "#C5E1A5";
    },

    format(data) {
      if (data) return moment(data).locale("pt").format("DD/MM/YYYY");
    },

    imunizacao_adicionada() {
      this.loadData();
      this.$snackbar.showMessage({
            show: true,
            color: "success",
            text: "Imunização adicionada com sucesso.",
            timeout: 4000,
          });

    },

    loadData: async function () {
      this.items = []
      let route =
        this.$store.state.tipo == "Veterinario"
          ? "http://localhost:7777/medico/imunizacao"
          : "http://localhost:7777/clinica/imunizacao";

      try {
        let response = await axios.post(
          route,
          {
            id: this.animal.id,
          },
          {
            headers: { Authorization: "Bearer " + store.getters.token },
          }
        );
        this.items = response.data;
      } catch (e) {
        console.log(e);
      }
    },
  },
  created() {
    this.loadData();
  },
};
</script>

<style>
.custom {
  width: auto;
  margin-left: auto;
}

.custom .v-pagination__navigation {
  height: 26px !important;
  width: 26px !important;
}

.custom .v-pagination__navigation .v-icon {
  font-size: 16px !important;
}

.custom .v-pagination__item {
  height: 26px !important;
  min-width: 26px !important;
  font-size: 0.85rem !important;
}
</style>