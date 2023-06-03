<template>
  <div>
    <v-container>
      <v-row>
        <v-col cols="2" class="flex-grow-0 flex-shrink-0">
          <template>
            <v-card flat>
              <v-card-title class="mb-0 pb-0">Curso</v-card-title>
              <v-card-text>
                <v-radio-group class="mt-2">
                  <v-radio label="Radio 1" value="1" color="laranja"></v-radio>
                  <v-radio label="Radio 2" value="2" color="laranja"></v-radio>
                  <v-radio label="Radio 3" value="3" color="laranja"></v-radio>
                </v-radio-group>
              </v-card-text>
            </v-card>
          </template>
        </v-col>
        <v-col cols="10" class="flex-grow-1 flex-shrink-0">
          <v-card>
            <v-card-title>
              <v-text-field
                v-model="buscar"
                color="laranja"
                append-icon="mdi-magnify"
                label="Buscar por nome"
                single-line
                hide-details
              ></v-text-field>
            </v-card-title>
            <!--single-select
          show-select
          @input="visualizarFichaIndividual()"-->
            <v-data-table
              :headers="cabecalhoTabela"
              :items="defesas"
              :items-per-page="10"
              :search="buscar"
              :loading="carregando"
              item-key="Ordem"
              loading-text="Carregando dados..."
              checkbox-color="laranja"
              @click:row="visualizarFichaIndividual"
            >
              ><template slot="progress">
                <v-progress-linear
                  color="laranja"
                  indeterminate
                ></v-progress-linear>
              </template>
              <template v-slot:[`item.Data`]="{ item }">
                {{ formatarData(item.Data) }}
              </template>
            </v-data-table>
          </v-card>
        </v-col>
      </v-row>
      <v-dialog v-model="dialog" max-width="450">
        <card-ficha-individual 
          :defesaSelecionada="defesaSelecionada"
          @fechar="dialog = false"
        />
      </v-dialog>
    </v-container>
  </div>
</template>

<script>
import CardFichaIndividual from "@/components/CardFichaIndividual.vue";

export default {
  name: "TabelaDefesas",

  data() {
    return {
      dialog: false,
      buscar: "",
      defesaSelecionada: {},
      defesas: [],
      carregando: true,
      cabecalhoTabela: [
        { text: "Nome", value: "Nome", filterable: true },
        { text: "Curso", value: "Curso", filterable: false },
        { text: "Programa", value: "Programa", filterable: false },
        { text: "Data", value: "Data", filterable: false },
      ],
    };
  },

  methods: {
    buscarDefesas() {
      const url = "http://thanos.icmc.usp.br:4567/api/v1/defesas";
      fetch(url)
        .then((data) => data.json())
        .then((response) => {
          this.defesas = response.items.map((item) => {
            return {
              ...item,
              Data: new Date(item.Data.split("/").reverse().join("-")),
            };
          });
          this.carregando = false;
        });
    },
    formatarData(data) {
      const dia = data.getDate().toString().padStart(2, "0");
      const mes = (data.getMonth() + 1).toString().padStart(2, "0");
      const ano = data.getFullYear().toString();
      return `${dia}/${mes}/${ano}`;
    },
    visualizarFichaIndividual(value) {
      console.log(value.Nome);

      let Nome = value.Nome;
      let Curso = value.Curso;
      let Programa = value.Programa;
      let Data = this.formatarData(value.Data);

      this.defesaSelecionada = { Nome, Curso, Programa, Data }
      this.dialog = true;
    },
  },

  beforeMount() {
    this.buscarDefesas();
  },

  components: {
    CardFichaIndividual,
  },
};
</script>


<style>
td.text-start:hover {
  cursor: pointer;
}
.btn-dialog {
  color: white !important;
  background-color: #ff609a !important;
}
</style>
