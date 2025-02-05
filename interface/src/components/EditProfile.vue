<template>
  <div>
    <v-container>
      <v-card flat color="#fafafa">
        <h3 class="pa-3">Dados de Acesso</h3>
        <v-divider></v-divider>
        <v-form ref="form" lazy-validation class="form">
          <v-row class="w-100" align="start">
            <v-col>
              <v-card class="h-100 mt-5" outlined>
                <v-list-item>
                  <v-list-item-content>
                    <v-col>
                      <p class="ma-0">Email</p>
                      <v-text-field
                        outlined
                        dense
                        :value="utilizador.email"
                        disabled
                      ></v-text-field>
                    </v-col>

                    <v-col>
                      <p class="ma-0">Palavra-passe</p>
                      <v-text-field
                        type="password"
                        placeholder="*****"
                        outlined
                        dense
                        color="#2596be"
                        v-model="password"
                      ></v-text-field>
                    </v-col>
                  </v-list-item-content>
                </v-list-item>
              </v-card>
            </v-col>
          </v-row>

          <h3 class="pa-3">Dados Pessoais</h3>
          <v-divider></v-divider>
          <v-row class="w-100" align="start">
            <v-col>
              <v-card class="h-100 mt-5" outlined>
                <v-list-item>
                  <v-list-item-content>
                    <v-col>
                      <p class="ma-0">Nome</p>
                      <v-text-field
                        outlined
                        color="#2596be"
                        dense
                        :rules="textRules"
                        v-model="utilizador.nome"
                      ></v-text-field>
                    </v-col>

                    <v-col>
                      <p class="ma-0">Morada</p>
                      <v-text-field
                        outlined
                        dense
                        color="#2596be"
                        v-model="utilizador.morada"
                      ></v-text-field>
                    </v-col>

                    <v-col>
                      <p class="ma-0">Freguesia</p>
                      <v-text-field
                        outlined
                        dense
                        color="#2596be"
                        :rules="textRules"
                        v-model="utilizador.freguesia"
                      ></v-text-field>
                    </v-col>

                    <v-col>
                      <p class="ma-0">Concelho</p>
                      <v-text-field
                        outlined
                        color="#2596be"
                        :rules="textRules"
                        dense
                        v-model="utilizador.concelho"
                      ></v-text-field>
                    </v-col>
                  </v-list-item-content>
                </v-list-item>
              </v-card>
            </v-col>
          </v-row>

          <h3 class="pa-3">Dados de Contacto</h3>
          <v-divider></v-divider>
          <v-row class="w-100" align="start">
            <v-col>
              <v-card class="h-100 mt-5" outlined>
                <v-list-item>
                  <v-list-item-content>
                    <v-col>
                      <p class="ma-0">Contacto telefónico</p>
                      <v-text-field
                        outlined
                        color="#2596be"
                        :rules="numberRules"
                        maxlength="9"
                        dense
                        v-model="utilizador.contacto"
                      ></v-text-field>
                    </v-col>

                    <v-col>
                      <p class="ma-0">Número de identificação fiscal</p>
                      <v-text-field
                        outlined
                        dense
                        v-model="utilizador.nif"
                        disabled
                        color="#2596be"
                      ></v-text-field>
                    </v-col>
                  </v-list-item-content>
                </v-list-item>
              </v-card>
            </v-col>
          </v-row>
        </v-form>
      </v-card>
      <v-row align="end" justify="end" class="w-100">
        <v-col cols="auto" class="ml-auto">
          <Cancelar :dialogs="cancelar" @clicked="close()"></Cancelar>
        </v-col>
        <v-col cols="auto">
          <v-btn color="#2596be" small dark @click="editarDados()"
            >Confirmar</v-btn
          >
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
import moment from "moment";
import axios from "axios";
import store from "@/store.js";
import Cancelar from "@/components/Dialogs/Cancel.vue";
export default {
  data: () => ({
    utilizador: {},
    password: "",
    dialogs: {},
    cancelar: {
      text: "a edição de dados",
      title: "edição de dados",
    },
    textRules: [
      (v) => {
        const pattern = /^[a-zA-Z\sÀ-ÿ]+$/;
        return (
          pattern.test(v) ||
          "Campo inválido. Insira apenas caracteres do alfabeto."
        );
      },
    ],
    numberRules: [
      (v) => {
        const pattern = /^[0-9]{9}$/;
        return pattern.test(v) || "Campo inválido. Insira 9 dígitos.";
      },
    ],
  }),
  components: {
    Cancelar,
  },
  methods: {
    format(data) {
      return moment(data).locale("pt").format("DD/MM/YYYY");
    },

    editarDados: async function () {
      let route =
        store.state.tipo == "Clinica"
          ? "http://localhost:7777/clinica/editar"
          : "http://localhost:7777/medico/preferencias";
      if (this.$refs.form.validate()) {
        try {
          if (this.password == "") this.password = this.utilizador.password;
          await axios.post(
            route,
            {
              email: store.state.email,
              nome: this.utilizador.nome,
              morada: this.utilizador.morada,
              concelho: this.utilizador.concelho,
              freguesia: this.utilizador.freguesia,
              contacto: this.utilizador.contacto,
              password: this.password,
            },
            {
              headers: {
                Authorization: "Bearer " + store.getters.token.toString(),
              },
            }
          );
          store.state.tipo == "Clinica"
            ? this.$router.push("/clinica/preferencias/")
            : this.$router.push("/medico/preferencias/");
          this.$snackbar.showMessage({
            show: true,
            color: "success",
            text: "Dados editados com sucesso.",
            timeout: 4000,
          });
        } catch (e) {
          console.log("erro: " + e);
          this.$snackbar.showMessage({
            show: true,
            color: "warning",
            text: "Ocorreu um erro no registo, por favor tente mais tarde!",
            timeout: 4000,
          });
        }
      } else {
        this.done = false;
        this.$snackbar.showMessage({
          show: true,
          color: "error",
          text: "Por favor preencha todos os campos.",
          timeout: 4000,
        });
      }
    },
    close() {
      store.state.tipo == "Clinica"
        ? this.$router.push("/clinica/preferencias")
        : this.$router.push("/medico/preferencias");
    },
  },

  created: async function () {
    let route =
      store.state.tipo == "Clinica"
        ? "http://localhost:7777/clinica"
        : "http://localhost:7777/medico";
    let response = await axios.post(
      route,
      {
        email: this.$store.state.email,
      },
      {
        headers: {
          Authorization: "Bearer " + store.getters.token.toString(),
        },
      }
    );

    this.utilizador = response.data;
  },
};
</script>

<style scoped>
.infos {
  text-align: start;
}
.respos {
  text-align: end;
  font-weight: bold;
}

.body-2 {
  font-size: 0.8rem !important;
}

.head {
  font-size: 2.75rem !important;
}

.font-weight-bold {
  font-size: 15px;
}
.font-weight-regular {
  font-size: 14px;
}

.font-weight-bold.col-sm-12.col-md-auto.col-auto {
  padding-bottom: 0;
}

.row.col.col-6 {
  margin-top: 0;
}
</style>