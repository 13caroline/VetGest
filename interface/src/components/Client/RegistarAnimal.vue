<template>
  <div>
    <v-row>
      <v-container fluid>
        <v-card class="mx-auto" width="70%" flat color="transparent">
          <v-card-title class="font-weight-regular text-uppercase mt-10">
            Registar um Novo Animal
          </v-card-title>
          <v-card-subtitle>
            <p class="subtitle">
              Por favor preencha o seguinte formulário
            </p></v-card-subtitle
          >

          <v-card-text>
            <v-form ref="form" v-model="valid" lazy-validation>
              <v-row>
                <v-col cols="5">
                  <div>
                    <p class="ma-0">Nome *</p>
                    <v-text-field
                      color="#2596be"
                      flat
                      outlined
                      dense
                      :rules="nameRules"
                      v-model="nome"
                    ></v-text-field>
                  </div>
                </v-col>
                <v-col cols="3">
                  <div>
                    <p class="ma-0">Número de Chip </p>
                    <v-text-field
                      color="#2596be"
                      flat
                      outlined
                      dense
                      v-model="chip"
                    ></v-text-field>
                  </div>
                </v-col>
                <v-col cols="4">
                  <div>
                    <p class="ma-0">Espécie *</p>
                    <v-select
                      class="font-weight-regular"
                      outlined
                      dense
                      color="#2596be"
                      label="Por favor selecione"
                      :items="itemsespecie"
                      :rules="nullRule"
                      v-model="especie"
                    >
                    </v-select>
                  </div>
                </v-col>
              </v-row>

              <v-row class="mt-n5">
                <v-col cols="3">
                  <div>
                    <p class="ma-0">Raça *</p>
                    <v-select
                      color="#2596be"
                      class="font-weight-regular"
                      outlined
                      dense
                      label="Por favor selecione"
                      v-model="raca"
                      v-if="especie == 'Canídeo'"
                      :items="racasCao"
                      :rules="nullRule"
                    >
                    </v-select>
                    <v-select
                      color="#2596be"
                      class="font-weight-regular"
                      outlined
                      dense
                      label="Por favor selecione"
                      v-model="raca"
                      v-else
                      :items="racasGato"
                      :rules="nullRule"
                    >
                    </v-select>
                  </div>
                </v-col>
                <v-col cols="2">
                  <div>
                    <p class="ma-0">Altura</p>
                    <v-text-field
                      class="font-weight-regular"
                      outlined
                      dense
                      color="#2596be"
                      suffix="cm"
                      v-model="altura"
                      :rules="alturaRules"
                    >
                    </v-text-field>
                  </div>
                </v-col>

                <v-col>
                  <div>
                    <p class="ma-0">Data de Nascimento *</p>
                    <v-menu
                      v-model="menu2"
                      :close-on-content-click="false"
                      :nudge-right="40"
                      transition="scale-transition"
                      offset-y
                      min-width="auto"
                    >
                      <template v-slot:activator="{ on, attrs }">
                        <v-text-field
                          v-model="date"
                          append-icon="fas fa-calendar-alt"
                          readonly
                          dense
                          outlined
                          v-bind="attrs"
                          v-on="on"
                          :rules="nullRule"
                        ></v-text-field>
                      </template>
                      <v-date-picker
                        v-model="date"
                        @input="menu2 = false"
                        locale="pt PT"
                        :max="new Date().toISOString().substr(0, 10)"
                      ></v-date-picker>
                    </v-menu>
                  </div>
                </v-col>

                <v-col>
                  <div>
                    <p class="ma-0">Sexo *</p>
                    <v-radio-group class="ma-0" v-model="sexo">
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
                  </div>
                </v-col>

                <v-col>
                  <div>
                    <p class="ma-0">Castração *</p>
                    <v-radio-group class="ma-0" v-model="castracao">
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
                  </div>
                </v-col>
              </v-row>

              <v-row class="mt-n5">
                <v-col cols="5">
                  <div>
                    <p class="ma-0">Cor *</p>
                    <v-select
                      color="#2596be"
                      flat
                      outlined
                      dense
                      label="Por favor selecione"
                      multiple
                      :items="itemscor"
                      v-model="cor"
                      :rules="nullRule"
                    ></v-select>
                  </div>
                </v-col>
                <v-col cols="4">
                  <div>
                    <p class="ma-0">Pelagem *</p>
                    <v-select
                      color="#2596be"
                      flat
                      outlined
                      dense
                      label="Por favor selecione"
                      multiple
                      :items="itemspelagem"
                      v-model="pelagem"
                      :rules="nullRule"
                    ></v-select>
                  </div>
                </v-col>
                <v-col cols="3">
                  <div>
                    <p class="ma-0">Cauda *</p>
                    <v-select
                      color="#2596be"
                      flat
                      outlined
                      label="Por favor selecione"
                      dense
                      :items="itemscauda"
                      :rules="nullRule"
                      v-model="cauda"
                    ></v-select>
                  </div>
                </v-col>
              </v-row>

              <v-row class="mt-n5">
                <v-col>
                  <div>
                    <p class="ma-0">Observações</p>
                    <v-textarea
                      outlined
                      color="#2596be"
                      rows="2"
                      clearable
                      clear-icon="fas fa-times-circle"
                      no-resize
                      v-model="observacoes"
                    ></v-textarea>
                  </div>
                </v-col>
              </v-row>
              <span class="ma-0">* Campos obrigatórios</span>
              <v-row align="end" justify="end">
                <v-btn color="#BDBDBD" small dark @click="dialog = true"
                  >Cancelar</v-btn
                >
                <v-btn
                  color="#2596be"
                  small
                  dark
                  class="ml-3"
                  @click="registaAnimal()"
                  >Registar</v-btn
                >
              </v-row>
            </v-form>
          </v-card-text>
        </v-card>
      </v-container>
    </v-row>
    <v-dialog v-model="dialog" persistent width="100%" max-width="460">
      <v-card>
        <v-card-title class="cancel">
          Cancelar registo de um novo animal
        </v-card-title>
        <v-card-text>
          Tem a certeza que pretende cancelar o registo?
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
            to="/cliente/inicio"
          >
            Sim
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
//import moment from 'moment';
import axios from "axios";
import store from "@/store.js";
export default {
  data: () => ({
    dialog: false,
    date: new Date().toISOString().substr(0, 10),
    menu2: false,
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
    racasCao: [
      "Affenpinscher",
      "Airedale Terrier",
      "Akbash",
      "Akita Inu",
      "American Toy Terrier",
      "Anglo Francês",
      "Barbado da Terceira",
      "Basenji",
      "Basset Hound",
      "Beagle",
      "Bernese",
      "Bichon Frise",
      "Bloodhound",
      "Bobtail",
      "Boerboel",
      "Boiadeiro Australiano",
      "Border Collier",
      "Borzoi",
      "Boston Terrier",
      "Boxer",
      "Braco Francês",
      "Braco Italiano",
      "Briard",
      "Brittany",
      "Bull Terrier",
      "Bulldog Americano",
      "Bulldog Inglês",
      "Cairn Terrier",
      "Cane Corso",
      "Caniche",
      "Cão d'Água Irlandês",
      "Cão d'Água Português",
      "Cavalier King Charles Spaniel",
      "Chesapeake Bay Retriever",
      "Chihuahua",
      "Chow Chow",
      "Clumber Spaniel",
      "Cocker Inglês",
      "Collie",
      "Cocker Americano",
      "Crista Chinês",
      "Csky Terrier",
      "Cuvac",
      "Dálmata",
      "Dandie Dinmont Terrier",
      "Drever",
      "Doberman",
      "Dogue Alemão",
      "Dogue Argentino",
      "Dogue de Bordeaux",
      "Dogue Canário",
      "Elkhund Norueguês",
      "Epagneul Breton",
      "Fila Brasileiro",
      "Fila de S.Miguel",
      "Fox Terrier",
      "Foxhound",
      "Gado Transmontano",
      "Galgo Afegão",
      "Galgo Polaco",
      "Golden Retriever",
      "Hokkaido",
      "Husky Siberiano",
      "Irish Wolfhound",
      "Ibizian Hound",
      "Irish Soft Coated Wheaten Terrier",
      "Jack Russel Terrier",
      "Keeshond",
      "Komondor",
      "Labrador Retriever",
      "Lakeland Terrier",
      "Lhasa Apso",
      "Leonberger",
      "Maltês",
      "Mastiff Inglês",
      "Mastim Napoletano",
      "Ovelheiro Gaúcho",
      "Papillon",
      "Pastor Alemão",
      "Pastor Australiano",
      "Pastor Belga",
      "Pastor Branco Suiço",
      "Pastor do Cáucaso",
      "Perdigueiro",
      "Pequinês",
      "Pitbull",
      "Podengo Português",
      "Pointer",
      "Pug",
      "Retriever de Pêlo Encaracolado",
      "Retriever de Pêlo Liso",
      "Rottweiler",
      "Rhodesian Ridgeback",
      "Samoiedo",
      "São Bernardo",
      "Schnauzer",
      "Scottish-Terrier",
      "Serra da Estrela",
      "Serra de Aires",
      "Setter Gordon",
      "Setter Inglês",
      "Setter Irlandês",
      "Shar Pei",
      "Shih-Tzu",
      "Sheepdog",
      "Spitz Alemão",
      "Staffordshire Bull Terrier",
      "Staffordshire Terrier Americano",
      "Terra Nova",
      "Teckel",
      "Tosa Inu",
      "Vizsla",
      "Veadeiro Pampeano",
      "Volpino Italiano",
      "Whippet",
      "Weimaraner",
      "West Highland White Terrier",
      "Yorkshire Terrier",
      "Xoloitzcuintli",
      "ZwergPinscher",
    ],
    racasGato: [
      "Abissínio",
      "American Wirehair",
      "Angorá Turco",
      "Asian",
      "Australian Mist",
      "Balinês",
      "Bobtail Americano",
      "Bobtail Japonês",
      "Bombay",
      "Burmês",
      "Burmilla",
      "Chartreux",
      "Cornish Rex",
      "Curl Americano",
      "Cymric",
      "Devon Rex",
      "Don Sphynx",
      "Egyptian Mau",
      "Europeu Comum",
      "Gato Bosques da Noruega",
      "German Rex",
      "Havana",
      "Khao Manee",
      "Korat",
      "Kurilian Bobtail",
      "LaPerm",
      "Longhair Britânico",
      "Maine Coon",
      "Manx",
      "Munchkin",
      "Neva Masquerade",
      "Ocicat",
      "Oriental",
      "Peterbald",
      "Pixiebob",
      "RagaMuffin",
      "Ragdoll",
      "Russian",
      "Sagrado da Birmânia",
      "Scottish Straight",
      "Selkirk Rex",
      "Seychellois",
      "Shorthair Americano",
      "Shorthair Britânico",
      "Siamês",
      "Siberiano",
      "Singapura",
      "Snowshoe",
      "Sokoke",
      "Somali",
      "Tonkinese",
      "Van Turco",
      "Vankedisi Turco",
    ],
    itemscauda: ["Comprida", "Curta", "Amputada"],
    itemsespecie: ["Canídeo", "Felídeo"],
    nome: "",
    chip: "",
    especie: "",
    altura: "",
    data: null,
    sexo: "",
    cor: "",
    raca: "",
    pelagem: "",
    castracao: false,
    cauda: "",
    observacoes: "",
    valid: true,
    done: false,
    nameRules: [
      (v) => !!v || "Insira o nome completo.",
      (v) => {
        const pattern = /^[a-zA-Z\sÀ-ÿ]+$/;
        return (
          pattern.test(v) ||
          "Nome inválido. Insira apenas caracteres do alfabeto."
        );
      },
    ],
    nullRule: [(v) => !!v || "Este campo não pode estar vazio."],
    alturaRules: [
      (v) => {
        const pattern = /^[0-9]+$/;
        return pattern.test(v) || "Altura inválida. Insira apenas dígitos.";
      },
    ],
  }),
  methods: {
    registaAnimal: async function () {
      if (this.$refs.form.validate()) {
        try {
          await axios.post(
            "http://localhost:7777/cliente/animal/registar",
            {
              cliente: {
                email: this.$store.state.email,
              },
              animal: {
                nome: this.nome,
                raca: this.raca,
                chip: this.chip,
                especie: this.especie,
                altura: this.altura,
                dataNascimento: this.date,
                sexo: this.sexo,
                cor: this.cor.toString(),
                pelagem: this.pelagem.toString(),
                cauda: this.cauda,
                castracao: this.castracao,
                observacoes: this.observacoes,
              },
            },
            {
              headers: {
                Authorization: "Bearer " + store.getters.token.toString(),
              },
            }
          );
          this.$router.push("/cliente/inicio");
          this.$snackbar.showMessage({
            show: true,
            color: "success",
            text: "Animal registado com sucesso.",
            timeout: 4000,
          });
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
  },
};
</script>

<style scoped>
.subtitle {
  color: #2596be;
}
</style>