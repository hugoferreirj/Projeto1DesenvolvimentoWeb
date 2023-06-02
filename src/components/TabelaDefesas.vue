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
        </v-col>
      </v-row>
      <v-dialog v-model="dialog" max-width="450">
        <v-card>
          <v-card-title class="mb-1">
            {{ defesaSelecionadaNome }}
          </v-card-title>
          <v-card-subtitle class="pb-0">
            {{ defesaSelecionadaData }}
          </v-card-subtitle>
          <v-card-text>
            <div class="font-weight-bold ms-1">Curso</div>
            <div class="ms-1">{{ defesaSelecionadaCurso }}</div>
            <div class="font-weight-bold ms-1">Programa</div>
            <div class="ms-1">{{ defesaSelecionadaPrograma }}</div>
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
  retornaCursosExistentes() {
    var cursos = [];
    for (var i = 0; i < this.defesas.length; i++) {
      if (cursos.include(this.defesas[i].Curso) == false) {
        cursos.push(this.defesas[i].Curso);
      }
    }
    return cursos;
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
