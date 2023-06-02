<template>
  <div>
    <v-container>
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
          v-model="defesaSelecionada"
          :headers="cabecalhoTabela"
          :items="defesas"
          :items-per-page="10"
          :search="buscar"
          :loading="carregando"
          item-key="Nome"
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
      <v-dialog v-model="dialog" max-width="500">
        <v-card>
          <v-card-title> {{ defesaSelecionadaNome }} </v-card-title>
          <!--<v-card-subtitle> {{ defesaSelecionadaData }} </v-card-subtitle>-->
          <v-card-text>
            Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do
            eiusmod tempor incididunt ut labore et dolore magna aliqua.
          </v-card-text>
          <v-card-actions>
            <v-btn class="btn-dialog" block @click="dialog = false"
              >Fechar</v-btn
            >
          </v-card-actions>
        </v-card>
      </v-dialog>
    </v-container>
  </div>
</template>

<script>
export default {
  name: "TabelaDefesas",
  data() {
    return {
      dialog: false,
      buscar: "",
      defesaSelecionadaNome: undefined,
      defesaSelecionadaData: undefined,
      defesaSelecionadaPrograma: undefined,
      defesaSelecionadaCurso: undefined,
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
      this.defesaSelecionadaNome = value.Nome;
      this.defesaSelecionadaData = this.formatarData(value.Data);
      this.defesaSelecionadaPrograma = value.Programa;
      this.defesaSelecionadaCurso = value.Curso;
      this.dialog = true;
    },
  },
  beforeMount() {
    this.buscarDefesas();
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
