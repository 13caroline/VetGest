<template>
  <div>
    <v-container>
      <v-card-title class="font-weight-bold text-uppercase">
        <h3 class="font-weight-regular text-uppercase mr-2">
          <v-icon small>fas fa-paw</v-icon>
          Editar dados de {{ animal.nome }}
        </h3>
      </v-card-title>
      <v-card-text>
        <v-form ref="form" v-model="valid" lazy-validation>
          <v-row>
            <v-col cols="auto" md="3" class="ml-4">
              <v-card width="200"  flat color="transparent">
                <v-img
                  :src="imagem"
                  aspect-ratio="1"
                  class="grey lighten-2"
                  cover
                >
                  <template v-slot:placeholder>
                    <v-row
                      class="fill-height ma-0"
                      align="center"
                      justify="center"
                    >
                      <v-progress-circular
                        indeterminate
                        color="grey lighten-5"
                      ></v-progress-circular>
                    </v-row>
                  </template>
                </v-img>
              </v-card>
              <v-row>
                <v-btn
                  class="body-2 mx-2 mt-4"
                  small
                  color="#2596be"
                  dark
                  @click="addFoto()"
                >
                  Alterar Fotografia
                  <v-icon small class="ml-4">fas fa-camera</v-icon>
                </v-btn>
              </v-row>
            </v-col>

            <v-col cols="12" md class="mt-10 mt-md-0">
              <v-row>
                <v-col cols="12" sm="4" class="py-0">
                  <p class="ma-0">Nome</p>
                  <v-text-field
                    color="#2596be"
                    :rules="nomeRules"
                    outlined
                    dense
                    disabled
                    v-model="animal.nome"
                  ></v-text-field>
                </v-col>
                <v-col cols="12" sm="3" class="py-0">
                  <p class="ma-0">Espécie</p>
                  <v-text-field
                    color="#2596be"
                    class="font-weight-regular"
                    flat
                    outlined
                    dense
                    disabled
                    v-model="animal.especie"
                  >
                  </v-text-field>
                </v-col>
                <v-col cols="12" sm="5" class="py-0">
                  <p class="ma-0">Raça</p>
                  <v-text-field
                    disabled
                    color="#2596be"
                    class="font-weight-regular"
                    outlined
                    dense
                    v-model="animal.raca"
                  >
                  </v-text-field>
                </v-col>
              </v-row>

              <v-row>
                <v-col cols="8" sm="4" class="py-0">
                  <p class="ma-0">Data de Nascimento</p>
                  <v-text-field
                    color="#2596be"
                    outlined
                    dense
                    disabled
                    v-model="animal.dataNascimento"
                  ></v-text-field>
                </v-col>
                <v-col sm="2" class="py-0">
                  <p class="ma-0">Altura</p>
                  <v-text-field
                    color="#2596be"
                    class="font-weight-regular"
                    flat
                    outlined
                    dense
                    suffix="cm"
                    :rules="alturaRules"
                    v-model="animal.altura"
                  >
                  </v-text-field>
                </v-col>
                <v-col cols="12" sm="6" class="py-0">
                  <p class="ma-0">Número de Chip</p>
                  <v-text-field
                    disabled
                    color="#2596be"
                    class="font-weight-regular"
                    outlined
                    dense
                    v-model="animal.chip"
                  >
                  </v-text-field>
                </v-col>
              </v-row>

              <v-row>
                <v-col cols="12" sm="5" class="py-0">
                  <p class="ma-0">Cor</p>
                  <v-select
                    color="#2596be"
                    flat
                    outlined
                    dense
                    :items="itemscor"
                    multiple
                    v-model="cor"
                  ></v-select>
                </v-col>

                <v-col cols="12" sm="4" class="py-0">
                  <p class="ma-0">Pelagem</p>
                  <v-select
                    color="#2596be"
                    flat
                    outlined
                    dense
                    :items="itemspelagem"
                    multiple
                    v-model="pelagem"
                  ></v-select>
                </v-col>

                <v-col cols="12" sm="3" class="py-0">
                  <p class="ma-0">Cauda</p>
                  <v-select
                    color="#2596be"
                    flat
                    outlined
                    dense
                    :items="itemscauda"
                    v-model="animal.cauda"
                  ></v-select>
                </v-col>
              </v-row>

              <v-row>
                <v-col class="py-0">
                  <p class="ma-0">Observações</p>
                  <v-textarea
                    outlined
                    color="#2596be"
                    rows="2"
                    clearable
                    clear-icon="fas fa-times-circle"
                    no-resize
                    v-model="animal.observacoes"
                  ></v-textarea>
                </v-col>
              </v-row>

              <v-row>
                <v-col cols="12" sm="auto" class="py-0">
                  <v-radio-group v-model="animal.sexo" row disabled>
                    <template v-slot:label>
                      <div>Sexo</div>
                    </template>
                    <v-radio value="Macho" color="#2596be">
                      <template v-slot:label>
                        <div class="body-2">Macho</div>
                      </template>
                    </v-radio>
                    <v-radio value="Fêmea" color="#2596be">
                      <template v-slot:label>
                        <div class="body-2">Fêmea</div>
                      </template>
                    </v-radio>
                  </v-radio-group>
                </v-col>
                <v-col cols="12" sm="auto" class="py-0">
                  <v-radio-group v-model="animal.castracao" row>
                    <template v-slot:label>
                      <div>Castração</div>
                    </template>
                    <v-radio :value="true" color="#2596be">
                      <template v-slot:label>
                        <div class="body-2">Sim</div>
                      </template>
                    </v-radio>
                    <v-radio :value="false" color="#2596be">
                      <template v-slot:label>
                        <div class="body-2">Não</div>
                      </template>
                    </v-radio>
                  </v-radio-group>
                </v-col>
              </v-row>

              <v-row align="end" justify="end">
                <v-col cols="auto" class="pr-0">
                  <v-btn color="#BDBDBD" small dark @click="dialog = true"
                    >Cancelar</v-btn
                  >
                </v-col>
                <v-col cols="auto" class="pl-0">
                  <v-btn
                    color="#2596be"
                    small
                    dark
                    class="ml-3"
                    @click="editarDados()"
                    >Confirmar</v-btn
                  >
                </v-col>
              </v-row>
            </v-col>
          </v-row>
        </v-form>
      </v-card-text>
    </v-container>
    <v-dialog v-model="dialog" persistent width="100%" max-width="460">
      <v-card>
        <v-card-title class="justify-center cancel">
          Cancelar edição de dados
        </v-card-title>
        <v-card-text>
          Tem a certeza que pretende cancelar a edição dos dados?
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            depressed
            large
            width="50%"
            dark
            color="#BDBDBD"
            @click="dialog = false"
          >
            Não
          </v-btn>
          <v-btn
            depressed
            large
            dark
            color="#2596be"
            width="50%"
            @click="cancelar()"
          >
            Sim
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <v-dialog v-model="dialog2" persistent width="100%" max-width="460">
        <v-card>
          <v-card-title>Adicionar fotografia</v-card-title>
          <v-card-text>
            <v-row justify="center" align="center">
              <div class="foto">
                <v-img
                  :src="url"
                  aspect-ratio="1"
                  class="grey lighten-2 ma-2 rounded"
                  cover
                >
                  <template v-slot:placeholder>
                    <v-row
                      class="fill-height ma-0"
                      align="center"
                      justify="center"
                    >
                    </v-row>
                  </template>
                </v-img>
              </div>
            </v-row>
            <v-row>
              <input
                type="file"
                id="file"
                ref="file"
                v-on:change="handleFileUpload()"
              />
            </v-row>
            <v-row align="end" justify="end">
              <v-col cols="auto">
                <v-btn
                  color="#BDBDBD"
                  class="mx-2"
                  small
                  dark
                  @click="dialog2 = false"
                  >Cancelar</v-btn
                >
                <v-btn color="#2596be" small dark @click="submitFile()"
                  >Registar</v-btn
                >
              </v-col>
            </v-row>
          </v-card-text>
        </v-card>
      </v-dialog>
  </div>
</template>

<script>
import axios from "axios";
import store from "@/store.js";

export default {
  props: ["id"],
  data: () => ({
    cor: [],
    pelagem: [],
url: null,
    file: "",
    imagem: "",
    dialog2: false,
    dialog: false,
    animal: {},
    itemscor: [
      "Amarelo",
      "Azul",
      "Branco",
      "Castanho",
      "Cinzento",
      "Creme",
      "Dourado",
      "Fulvo",
      "Malhado",
      "Preto",
      "Vermelho",
    ],
    itemspelagem: [
      "Comprida",
      "Média",
      "Curta",
      "Lisa",
      "Ondulada",
      "Encaracolada",
      "Cerdosa",
    ],
    itemscauda: ["Comprida", "Curta", "Amputada"],
    valid: true,
    done: false,
    nomeRules: [
      (value) => {
        const pattern = /^([a-zA-Z]+)$/;
        return pattern.test(value) || "O nome só pode conter letras";
      },
    ],
    alturaRules: [
      (value) => {
        const pattern = /^([0-9]+)$/;
        return pattern.test(value) || "Introduza apenas dígitos";
      },
    ],
  }),
  methods: {
    handleFileUpload() {
      this.file = this.$refs.file.files[0];
      this.url = URL.createObjectURL(this.file);
    },
    addFoto() {
      this.dialog2 = true;
    },
    submitFile() {
      let formData = new FormData();

      formData.append("imageFile", this.file);
      formData.append("userid", this.animal.id);
      try {
        axios.post("http://localhost:7777/cliente/adicionaFoto", formData, {
          headers: {
            Authorization: "Bearer " + store.getters.token.toString(),
          },
        });
        this.imagem = this.url;
        this.dialog = false;
        this.$emit("clicked", {
            text: "Fotografia adicionada com sucesso",
            color: "success",
            snackbar: "true",
            timeout: 4000,
          });
          this.dialog2=false;
      } catch (e) {
        this.$snackbar.showMessage({
          show: true,
          color: "success",
          text: "Ocorreu um erro, por favor tente mais tarde!",
          timeout: 4000,
        });
      }
    },
    editarDados: async function () {
      if (this.$refs.form.validate()) {
        try {
          await axios.post(
            "http://localhost:7777/cliente/editar/animal/" + this.id,
            {
              cliente: {
                email: store.state.email,
              },
              animal: {
                nome: this.animal.nome,
                raca: this.animal.raca,
                dataNascimento: this.animal.dataNascimento,
                sexo: this.animal.sexo,
                especie: this.animal.especie,
                altura: this.animal.altura,
                cor: this.cor.toString(),
                pelagem: this.pelagem.toString(),
                cauda: this.animal.cauda,
                chip: this.animal.chip,
                observacoes: this.animal.observacoes,
                castracao: this.animal.castracao,
              },
            },
            {
              headers: {
                Authorization: "Bearer " + store.getters.token.toString(),
              },
            }
          );

          this.$snackbar.showMessage({
            show: true,
            color: "success",
            text: "Dados editados com sucesso.",
            timeout: 4000,
          });
          this.$router.push("/cliente/animal/" + this.id);
        } catch (e) {
          this.$snackbar.showMessage({
            show: true,
            color: "warning",
            text: "Ocorreu um erro no registo, por favor tente mais tarde!",
            timeout: 4000,
          });
        }
      } else {
        this.$snackbar.showMessage({
          show: true,
          color: "error",
          text: "Por favor preencha todos os campos.",
          timeout: 4000,
        });
        this.done = false;
      }
    },
    cancelar() {
      this.$router.push("/cliente/animal/" + this.id);
    },
  },
  created: async function () {
    let response = await axios.post(
      "http://localhost:7777/cliente/animal/" + this.id,
      {
        email: store.state.email,
      },
      {
        headers: {
          Authorization: "Bearer " + store.getters.token.toString(),
        },
      }
    );
    this.animal = response.data.animal;
    this.cor = response.data.animal.cor.split(",");
    this.pelagem = response.data.animal.pelagem.split(",");
    if (this.animal.observacoes.length == 0)
      this.animal.observacoes = "Sem observações";
    this.imagem = response.data.animal.image
        ? "data:image/jpeg;charset=utf-8;base64," + response.data.animal.image
        : require("@/assets/image_placeholder.png");
  },
};
</script>

<style>
.foto {
  width: 200px;
}
</style>