<template>
  <div class="divMae">
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
        <v-data-table
          v-model="defesaSelecionada"
          :headers="cabecalhoTabela"
          :items="defesas"
          :items-per-page="10"
          :search="buscar"
          :loading="carregando"
          single-select
          show-select
          @input="visualizarFichaIndividual()"
          item-key="Nome"
          loading-text="Carregando dados..."
          checkbox-color="laranja"
        >
          ><template slot="progress">
            <v-progress-linear
              color="laranja"
              indeterminate
            ></v-progress-linear> </template
          ><template v-slot:[`item.Data`]="{ item }">
            {{ formatarData(item.Data) }}
          </template>
        </v-data-table>
      </v-card>
    </v-container>
  </div>
</template>

<script>
export default {
  name: "TabelaDefesas",
  data() {
    return {
      buscar: "",
      defesaSelecionada: undefined,
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
    visualizarFichaIndividual() {
      console.log(this.defesaSelecionada[0]);
    },
  },
  beforeMount() {
    this.buscarDefesas();
  },
};
</script>


<style>
.divMae {
  background: #ffd152;
}
</style>
