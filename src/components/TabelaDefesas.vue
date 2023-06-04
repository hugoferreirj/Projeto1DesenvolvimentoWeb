<template>
  <div>
    <v-container>
      <v-row>
        <v-col cols="2" class="flex-grow-0 flex-shrink-0">
          <template>
            <v-card flat>
              <v-card-title class="mb-0 pb-0">Cursos</v-card-title>
              <v-card-text>
                <v-radio-group class="mt-2" v-model="cursoSelecionado">
                  <v-radio v-for="curso in filtrosRadio['Curso']"
                    v-bind:key="curso" 
                    v-bind:label="curso" 
                    v-bind:value="curso"
                    color="laranja">
                  </v-radio>
                  <v-btn class="btn-filter" block @click="cursoSelecionado=undefined">Limpar</v-btn>
                </v-radio-group>
              </v-card-text>
            </v-card>
            <v-card flat>
              <v-card-title class="mb-0 pb-0">Programas</v-card-title>
              <v-card-text>
                <v-radio-group class="mt-2" v-model="programaSelecionado">
                  <v-radio v-for="programa in filtrosRadio['Programa']"
                    v-bind:key="programa" 
                    v-bind:label="programa" 
                    v-bind:value="programa"
                    color="laranja">
                  </v-radio>
                  <v-btn class="btn-filter" block @click="programaSelecionado=undefined">Limpar</v-btn>
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
          :dicionarioCursos="dicionarioCursos"
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

      dicionarioCursos: {
        ME: 'Mestrado',
        DO: 'Doutorado',
        DD: 'Doutorado Direto',
      },
      
      filtrosRadio: {},
      cursoSelecionado: "",
      programaSelecionado: "",
    };
  },

  computed: {
    cabecalhoTabela(){
      return [
        { text: "Nome", value: "Nome", filter: (value) => {
            return !this.buscar || value.toLocaleLowerCase().includes(this.buscar.toLocaleLowerCase());
          }  
        },
        { text: "Curso", value: "Curso", 
          filter: (value) => {
            return !this.cursoSelecionado || value == this.cursoSelecionado;
          } 
        },
        { text: "Programa", value: "Programa", 
          filter: (value) => {
            return !this.programaSelecionado || value == this.programaSelecionado;
          } 
        },
        { text: "Data", value: "Data", filterable: false },
      ];
    }
  },

  methods: {
    carregarFiltrosRadio(campos){
      campos.forEach(campo => {
        this.filtrosRadio[campo] = []
      });

      this.defesas.forEach(defesa => {
        campos.forEach(campo => {
          if( !this.filtrosRadio[campo].includes(defesa[campo]) ){
            this.filtrosRadio[campo].push(defesa[campo]);
          }
        });
      });
    },
    async buscarDefesas() {
      const url = "http://thanos.icmc.usp.br:4567/api/v1/defesas";

      try{
        let data = await fetch(url);
        let response = await data.json();

        this.defesas = response.items.map((item) => {
          return {
            ...item,
            Data: new Date(item.Data.split("/").reverse().join("-")),
          };
        });

        this.carregarFiltrosRadio(["Curso", "Programa"]);

      }catch(e){
        console.log("Erro ao carregar as defesas!", e);
      }

      this.carregando = false;
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
.btn-filter {
  color: white !important;
  background-color: #ff609a !important;
}
</style>
